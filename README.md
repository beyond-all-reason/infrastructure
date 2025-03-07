# Infrastructure

> [!IMPORTANT]
> Work in progress, *nothing* useful yet.

This repository contains **[documentation](https://beyond-all-reason.github.io/infrastructure/)**
and **[issue tracking](https://github.com/beyond-all-reason/infrastructure/issues)**
for Beyond All Reason (BAR) Infrastructure area.

See the [https://beyond-all-reason.github.io/infrastructure/](https://beyond-all-reason.github.io/infrastructure/)
build from this repository for more information.

## Documentation development

First set up the Python project and install required dependencies:

```
$ python3 -m venv .pyenv
$ curl -fsSL https://d2lang.com/install.sh | sh -s -- --tala --method standalone --prefix $(pwd)/.d2/
$ source .envrc
$ pip install -r requirements.txt
```

Then run

```
$ mkdocs serve
```

to start local server that watches all changes to the source files
and rebuilds documentation.
