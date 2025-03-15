# Infrastructure

Documentation (https://beyond-all-reason.github.io/infrastructure) and issue tracking for Beyond All Reason Infrastructure area.

## Documentation development

First set up the Python project and install required dependencies:

```
$ python3 -m venv .venv
$ curl -fsSL https://d2lang.com/install.sh | sh -s -- --tala --method standalone --prefix $(pwd)/.d2/
$ source .envrc
$ pip install -r requirements.txt
```

Then run

```
$ mkdocs serve
```

to start local server that watches all changes to the source files and rebuilds documentation.
