# prontload

Refresh [Pronto](https://github.com/ProteinsWebTeam/pronto/) with the latest data from InterPro, GOA, and UniProt.

## Installation

Get the latest version of the code and install requirements (Python 3.4+):

```sh
$ git clone https://github.com/ProteinsWebTeam/prontload.git
$ cd prontload/
$ python setup.py install
```

## Configuration

Create or edit the `config.json` file to set the following settings:

* `dsn`: Oracle DSN in the `user/password@host:port/service` format.
* `schema`: Oracle schema where to create/refresh tables. Be sure that the user defined in `dsn` has the right priviles (e.g. `CREATE TABLE`, `DROP ANY TABLE`, etc.).
* `max_gap`: Used to group proteins by their match structure. Historically set to `20`.

## Database links

The following public database links are required:
- `GOAPRO`
- `SWPREAD`

## Run

The only requirement argument is the path to `config.json`.

```sh
prontload config.json [OPTIONS]
```

Available options are:

| Option          | Description                                                                    |
| ------------- --|--------------------------------------------------------------------------------|
| -s, --steps     | space-separated list of steps to run (default: all)                            |
| -t, --tmpdir    | directory for temporary files (default: `/tmp/`)                               |
| -p, --processes | number of processes to use in the **method2protein** step (default: 1)         |
| -o, --output    | output file for the Swiss-Prot descriptions report for curators                |
| --verbose       | display additional (debug) logging messages                                    |

### Steps

| Name          | Description                                                                    |
| ------------- |--------------------------------------------------------------------------------|
| clear         | Drop all tables from the current schema (skipped unless explicitly called)     |
| databases     | Copy database info (e.g. name, version) from InterPro                          |
| comments      | Copy Swiss-Prot comments from UniProt                                          |
| enzymes       | Copy EC numbers ([ENZYME](https://enzyme.expasy.org/)) FROM UniProt            |
| annotations   | Copy protein GO annotations from GOA                                           |
| publications  | Copy publications from GOA                                                     |
| terms         | Copy GO terms from GOA                                                         |
| matches       | Copy protein matches from InterPro                                             |
| signatures    | Copy member database signatures from InterPro                                  |
| taxa          | Copy taxonomy data from InterPro                                               |
| proteins      | Copy proteins from InterPro                                                    |
| descriptions  | Copy protein descriptions from UniProt                                         |
| method2protein| Cross member database signatures and proteins, then run descriptions           |
| report        | Export gained/lost protein descriptions per InterPro entries                   |
| copy          | Export the current schema to an other to prevent downtime during updates       |
