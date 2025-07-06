# Runtime Infrastructure

On this page, we describe the components of the "Runtime" infrastructure: roughly what players interact with in a *direct* way to play local and multiplayer games, browse the website, etc.

All components of the runtime infrastructure also need to fetch different assets and configurations to function, and that part is described on the [Assets Serving](assets_serving.md) page.

> [!IMPORTANT]
> This page covers how we the runtime infrastructure looks like *right now*. The large strategic [New Lobby Client project](new_client.md) is going to change a lot of this, and makes some existing runtime components in maintanance mode.

## System diagram

> [!TIP]
> Elements with ⓘ on the diagram have tooltips. Hover over them to see more information.

![Diagram](diagrams/current_infra.d2){pad=10}

### Player local flow

The main way players interact with the game is by launching and playing the game.

The entry point of the interaction is [Spring Launcher](https://github.com/beyond-all-reason/spring-launcher), which is installed by the user [via installer](https://github.com/beyond-all-reason/BYAR-Chobby/releases) and is responsible for downloading all the other components of the game directly and via [pr-downloader](https://github.com/beyond-all-reason/pr-downloader) distributed with the launcher.

The launcher then starts the Recoil engine process with the "Menu" configured to be [Chobby](https://github.com/beyond-all-reason/BYAR-Chobby), which has the UI to log in, start a single-player game, etc. Chobby itself can't do much because it's written in Lua and constrained to use the engine APIs, but it has support for opening network sockets. For any advanced functions, e.g., Discord rich presence, triggering map downloads, it calls the launcher via the local network interface.

Chobby connects to the lobby server ([teiserver](https://github.com/beyond-all-reason/teiserver/)) using the [SpringLobby protocol](https://github.com/spring/LobbyProtocol) for multiplayer features. It's responsible for starting the game in the same engine instance it's running in. For details about starting the game in the engine, see [the article in Engine documentation](https://beyond-all-reason.github.io/RecoilEngine/articles/technicalities-of-starting-a-match).

### Multiplayer lobbies

[teiserver](https://github.com/beyond-all-reason/teiserver/) is the central component that Chobby and game servers connect to. It handles account management, rating, moderation, chat, and overall passing messages between different components.

The one thing that teiserver doesn't do is *create new rooms*. The rooms are created by [SPADS](https://github.com/Yaribz/SPADS) running on the Game Server. There is a single room per SPADS process, with a single special SPADS process used to start and stop new instances (see the diagram in the section below for details).

The lobby room logic is then split between teiserver and SPADS (It's something that is player visible: `$` vs `!` commands). SPADS contains the vast majority of lobby room logic and is responsible for starting the multiplayer game: starting the special engine dedicated instance and sending players the connection parameters.

SPADS communicates with the engine running on the server using the "Autohost" protocol and has admin privileges to execute different commands, e.g., the `!stop` command is executed in SPADS, and it's SPADS that resolves the vote and decides whether to stop the battle or not.

### Auxiliary services

Players also have access to the main website: https://www.beyondallreason.info/. The website is created in Webflow, but it also contains `<iframe>` with pages from https://bar-rts.com/ (BAR Live Services) that contain more interactive parts: replay browser, list of active battles, etc. BAR Live Services is a custom [frontend](https://github.com/beyond-all-reason/bar-live-services) and [backend](https://github.com/beyond-all-reason/bar-db) hosted on a dedicated VM, with PostgreSQL, etc.

### Game Server

Below diagram shows the Game Server setup in more detail.

The game server is managed by an Ansible playbook: https://github.com/beyond-all-reason/ansible-spads-setup

![Diagram](diagrams/current_game_server.d2){pad=10}
