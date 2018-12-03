# Bitcoin DAppnode package

[![DAppNodeStore Available](https://img.shields.io/badge/DAppNodeStore-Available-brightgreen.svg)](http://my.admin.dnp.dappnode.eth/#/installer/bitcoin.dnp.dappnode.eth)


This package makes it easy to deploy a [bitcoin](https://bitcoin.org) node in a DAppNode, for this purpose the [bitcoind](https://bitcoin.org/es/descargar) daemon is used.

## Prerequisites

- git

   Install [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) commandline tool.

- docker

   Install [docker](https://docs.docker.com/engine/installation). The community edition (docker-ce) will work. In Linux make sure you grant permissions to the current user to use docker by adding current user to docker group, `sudo usermod -aG docker $USER`. Once you update the users group, exit from the current terminal and open a new one to make effect.

- docker-compose

   Install [docker-compose](https://docs.docker.com/compose/install)
   
**Note**: Make sure you can run `git`, `docker ps`, `docker-compose` without any issue and without sudo command.


## Buidling

`docker-compose build`

## Running

### Start

`docker-compose up -d`

### View logs

`docker-compose logs -f`

### Stop

`docker-compose down`

## Environmet variables

You can edit the `docker-compose.yml` and add extra options, such as:
```
| name | default |
| ---- | ------- |
| BTC_RPCUSER | dappnode |
| BTC_RPCPASSWORD | dappnode |
| BTC_TXINDEX | 0 |

by default BTC_TXINDEX is 0, but the installation of the DAppNodePackage will set this value to 1, since it's a recommended value for other applications.
```

## Note

This is early stage software
