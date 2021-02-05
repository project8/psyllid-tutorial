# Psyllid Tutorial

## Objectives

* Run a RabbitMQ broker to setup a Dripline mesh
* Run Psyllid
* Start a run
* Start a run with an altered configuration

## Requirements

* Git
* Docker and docker-compose
* Text editor
* Three terminals
* Browser (optional)

## Instructions

### Preparation

1. (Host) Create a directory for the tutorial, and clone the psyllid-tutorial repository.
    ```
    mkdir tutorial
    cd tutorial
    git clone git@github.com:project8/psyllid-tutorial
    ```
    All three terminal windows should be in the `tutorial` directory.
1. (Host) Copy the files we'll need for the tutorial
	```
	cp psyllid-tutorial/str_1ch_dataprod.yaml .
	cp psyllid-tutorial/docker-compose.yaml .
	```
1. (Host) Edit `docker-compose.yaml`
    * `services/psyllid/volumes/[item 0]` host path should point to the tutorial directory on your machine
1. (Host, terminal 1) Start the RabbitMQ broker
    ```
    docker-compose up rabbit_broker
    ```
1. (Host, browser; optional) Open the RabbitMQ management page. URL: `localhost:15672`.  You'll be able to verify that Psyllid is connected to the broker and you can see that the queue has indeed received the requests you send.

### Run Psyllid

1. (Host, terminal 2) Start the Psyllid container.
    ```
    docker-compose up psyllid
    ```
1. (Host, terminal 3) Send ping to Psyllid.  We'll use the container in which Psyllid is running to do this.  With the `docker ps` command below, get the name of the container and insert it in the `docker exec` command that follows.
    ```
    docker ps
    docker exec -it [container name] /bin/bash
    ```
    The container will start up.  From in the container source the setup script, then issue the ping.
    ```
    source /usr/local/p8/compute/v0.10.1/setup.sh
    dl_agent -b broker psyllid ping cmd
    ```
    Ensure that you received a response from Psyllid that says "Hello, Dripline."
1. (Host, terminal 3) Send the run-start request to Psyllid
    ```
    dl_agent -b broker psyllid run
    ```
    The run will take one second and leave the output egg file in the `/tutorial` directory.  If you want you can look at that file with Katydid or any HDF5 reader.

### Shutdown and exit containers

1. Tell Psyllid to quit
    ```
    dl_agent -b broker psyllid quit-psyllid cmd
    ```
    Psyllid will shutdown and the container will exit.
1. (Host, terminal 2) Quit the RabbitMQ broker by pressing `ctrl-c`.
