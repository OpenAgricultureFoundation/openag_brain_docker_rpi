# openag\_brain\_docker\_rpi

This repository provides a docker-compose configuration for running the
[openag\_brain](http://github.com/OpenAgInitiative/openag_brain) project on a
Raspberry Pi.

## Install

System requirements:

- [Raspberry Pi 3](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/) with [Raspbian Jessie](https://www.raspberrypi.org/downloads/raspbian/) installed.
- [Arduino Mega](https://www.arduino.cc/en/Main/ArduinoBoardMega)

Before you start:

- Plug your Arduino Mega into one of the Raspberry Pi's USB ports.
- Connect via SSH, or open a terminal session to your Raspberry Pi. 

First, clone this repository on your Raspberry Pi:

    git clone http://github.com/OpenAgInitiative/openag_brain_docker_rpi
    cd openag_brain_docker_rpi

Now, install `docker` and `docker-compose`. This repository has a script that
does this for you. It leverages a repository set up by
[Hypriot](http://blog.hypriot.com/) which provides the necessary packages.

    sh install_docker.sh

After installing `docker`, you will have to refresh your user groups in order to contact the docker daemon. The easiest way to do this is to restart the Raspberry Pi (or if connected via SSH, log out and log back in).

Now, the project can be started as follows:

    docker-compose up -d

2 Docker containers will be started in the background and will persist across
reboots. (Note if you don't have your Arduino connected, you will see an error. If this happens, connect the Arduino and run the command again).

## Developing

See which containers are running:

    docker ps

The install script creates and runs 2 docker containers. You should see them listed when running `docker ps`:

    openagbraindockerrpi_brain_1
    openagbraindockerrpi_db_1

Stoping and starting the containers is done in the usual way:

    docker-compose start
    docker-compose stop
    docker-compose restart

You can get logs for the containers via docker's logging command:

    docker logs -f openagbraindockerrpi_brain_1