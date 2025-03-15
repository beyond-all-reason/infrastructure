# New Client and Protocol

BAR is working towards a [Steam release](https://www.beyondallreason.info/development/steam-release) to provide a modern and stable player experience. One of the requirements for the release is a new client that will replace [Chobby] and a new protocol, Tachyon, which will work together to enable multiple improvements.

## Overview

> [!IMPORTANT]
> This overview assumes some familiarity with the current infrastructure setup as described on the [Runtime Infrastructure](current_infra.md) page.

![Diagram](diagrams/new_client.d2){pad=10}

We can start with the protocol. All communication that currently uses the [SpringLobby Protocol] will switch to the new [Tachyon Protocol]. The SpringLobby Protocol was very successful in the past in the Spring RTS ecosystem, but it imposes many limitations on things that we would like to achieve in the future.

On the client side, we are deprecating the in-engine lobby, [Chobby], and [spring-launcher](https://github.com/beyond-all-reason/spring-launcher) with a single [Electron]-based application: [New Client, also known as BAR Lobby](https://github.com/beyond-all-reason/bar-lobby). Because the New Client is based on web technologies, it should be much easier to develop, and more people are familiar with this technology (similar argument for the work of adapting [RmlUi](https://github.com/mikke89/RmlUi) for in-game UI). Moreover, being a more native application, the New Client has access to full Operating System APIs, network APIs, and so on, which allows us to deprecate the launcher.

We are also deprecating the [SpringLobby Protocol] usage for game servers. In the [Tachyon Protocol], the game server component has different responsibilities than in SpringLobby: the battle room management is no longer shared between Teiserver and [SPADS], which leads to bugs, but will be fully owned by Teiserver. That means that we won't be able to use [SPADS], but we opted to develop a new [Recoil Autohost](https://github.com/beyond-all-reason/recoil-autohost) component.

## Development

There is ongoing parallel work on all four components of the project: protocol, new client, Teiserver, and Recoil Autohost. The protocol is a central piece that requires the most coordination between project participants. Most development conversations are happening on Discord in dedicated channels, primarily [#tachyon-protocol](https://discord.com/channels/549281623154229250/943582636679520256) and [#new-client](https://discord.com/channels/549281623154229250/927564746104905728).

### New Client

The roadmap with milestones is available at: https://github.com/beyond-all-reason/bar-lobby/wiki/Roadmap. This roadmap dictates the priorities for the development of all the other components.

*Technologies*: [TypeScript], [Electron], [Vue] <br>
*Repository*: https://github.com/beyond-all-reason/bar-lobby

### Teiserver

Most of the work involves implementing the Tachyon protocol endpoints. Relevant [GitHub issues](https://github.com/beyond-all-reason/teiserver/issues) are tagged with the `tachyon` tag.

*Technologies*: [Elixir](https://elixir-lang.org/) <br>
*Repository*: https://github.com/beyond-all-reason/teiserver

### Protocol

The protocol changes are driven primarily by the work in other components: if the new client needs some feature and there isn't an endpoint that provides that information, somebody volunteers to design it, and then it goes through a review process to agree on the schema, for example: [Party specification Pull Request](https://github.com/beyond-all-reason/tachyon/pull/53).

*Technologies*: [JSON Schema] <br>
*Repository*: https://github.com/beyond-all-reason/tachyon

### Recoil Autohost

Like the new client, this is an entirely new component that already has some very basic functionality implemented. However, there are still quite a few missing features that need to be addressed to make it production quality. Known missing work items are broken down into [GitHub issues](https://github.com/beyond-all-reason/recoil-autohost/issues).

*Technologies*: [TypeScript], [Node.js] <br>
*Repository*: https://github.com/beyond-all-reason/recoil-autohost

[SpringLobby Protocol]: https://github.com/spring/LobbyProtocol
[Tachyon Protocol]: https://github.com/beyond-all-reason/tachyon
[Chobby]: https://github.com/beyond-all-reason/BYAR-Chobby
[TypeScript]: https://www.typescriptlang.org/
[Electron]: https://www.electronjs.org/
[Node.js]: https://nodejs.org/
[JSON Schema]: https://json-schema.org/
[Vue]: https://vuejs.org/
[SPADS]: https://github.com/Yaribz/SPADS
