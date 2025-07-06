# Welcome

This documentation site describes the components of the [Beyond All Reason](https://www.beyondallreason.info/) (BAR) Infrastructure and how they connect to each other.

On a very high level:

```d2
Infrastructure {
  near: top-left
  style {
    fill: transparent
    border-radius: 10
    stroke-dash: 4
  }

  Game Online Services {
    shape:cloud
  }

  Game Servers {
    style.multiple: true
  }

  Lobby Client <-> _.Recoil Engine
  Lobby Server <-> Lobby Client
  Game Servers <-> Lobby Server
  Game Servers <-> _.Recoil Engine

}

Recoil Engine {
  near: top-right

  Beyond All Reason Game
}
```

Infrastructure contains all the software, configuration, and resources that support the running of the [Beyond All Reason](https://github.com/beyond-all-reason/Beyond-All-Reason) game on the [Recoil Engine](https://github.com/beyond-all-reason/RecoilEngine), which includes: lobby server, launcher, game files hosting, multiplayer game servers, server management, and more.

Please explore the site for any information that you are interested in. If you are just starting, we recommend looking at the [more detailed system overview](current_infra.md). If you would like to help out, check out the [contributing page](contributing.md).

Happy reading 🙂!
