# Psyllid Tutorial

## Objectives

* Run a RabbitMQ broker to setup a Dripline mesh
* Run Psyllid
* Start a run
* Start a run with an altered configuration

## Requirements

* Git
* Docker
* Text editor
* Three terminals
* Browser (optional)

## Instructions

### Preparation

1. (Host) Create a directory for the tutorial, and clone the psyllid (don't need submodules) and psyllid-tutorial repositories.
    ```
    mkdir tutorial
    cd tutorial
    git clone git@github.com:project8/psyllid
    git clone git@github.com:project8/psyllid-tutorial
    ```
    All three terminal windows should be in the `tutorial` directory.
1. (Host) Copy the files we'll need for the tutorial
	```
	cp psyllid/examples/str_1ch_dataprod.yaml .
	cp psyllid-tutorial/docker-compose.yaml .
	```
1. (Host) Edit `str_1ch_dataprod.yaml`:
    * `dripline/host` should be `broker`
1. (Host) Edit `docker-compose.yaml`
    * `services/psyllid/volumes/[item 0]` host path should point to the tutorial directory on your machine
1. (Host, terminal 1) Start the RabbitMQ broker
    ```
    docker-compose up rabbit_broker
    ```
1. (Host, browser; optional) Open the RabbitMQ management page. URL: `localhost:15672`
### Run Psyllid