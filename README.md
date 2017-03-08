# openag\_brain\_docker\_rpi

This repository provides a docker-compose configuration for running the
[openag\_brain](http://github.com/OpenAgInitiative/openag_brain) project on a
Raspberry Pi.

## Demo branch instructions

### Installing `openag_python`

You'll need [openag_python](https://github.com/OpenAgInitiative/openag_python.git)
to flash the Arduino with the firmware configuration. You can install it
on the Arduino, or a laptop. Be sure to install it with the `[flash]` flag
so you get all the microcontroller flashing tools.

    git clone https://github.com/OpenAgInitiative/openag_python.git
    cd openag_python
    pip install -e .[flash]

### Flashing the Arduino

On the machine that you've installed `openag_python`, clone the firmware
description files.

    git clone http://github.com/openaginitiative/openag_launch -b demo

Now that you've got the `demo` branch launch files, you can flash the Arduino
with the demo firmware:

    openag firmware flash -t upload -p ros -f ~/openag_launch/personal_food_computer_v2.yaml

### Standing up the Docker containers

In the `demo` branch of this repository, `docker-compose.yml` will point to the
`openag/rpi_brain:demo` tag. To bring up the project, just:

    cd ~/openag_brain_docker_rpi
    docker-compose u

## Install

System requirements:

- [Raspberry Pi 3](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/)
  or [Raspberry Pi 2 Model B](https://www.raspberrypi.org/products/raspberry-pi-2-model-b/)
  with [Raspbian Jessie](https://www.raspberrypi.org/downloads/raspbian/) installed.
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

After installing `docker`, you will have to refresh your user groups in order
to contact the docker daemon. The easiest way to do this is to restart the
Raspberry Pi (or if connected via SSH, log out and log back in).

## Running

Once installed, the project can be started by using the `docker-compose`
command. There are 2 docker-compose files in this repository:

- `docker-compose.yml` contains a base configuration. It doesn't do much, but
  serves as a good template for creating compose files.
- `personal_food_computer_v2.yml` contains a configuration for the [Personal
  Food Computer](http://wiki.openag.media.mit.edu/food_computer_2).

These can be used as follows:

    docker-compose -f personal_food_computer_v2.yml up -d

2 Docker containers will be started in the background and will persist across
reboots. (Note if you don't have your Arduino connected, you will see an error.
If this happens, connect the Arduino and run the command again).

## Working with Docker

See which containers are running:

    docker ps

The install script creates and runs 2 docker containers. You should see them
listed when running `docker ps`:

    openagbraindockerrpi_brain_1
    openagbraindockerrpi_db_1

Stoping and starting the containers is done in the usual way:

    docker-compose start
    docker-compose stop
    docker-compose restart

You can get logs for the containers via docker's logging command:

    docker logs -f openagbraindockerrpi_brain_1

## Working with the ROS Container

OpenAg Brain is powered by [ROS](http://www.ros.org/). Sometimes, you might
want to interact with ROS in the Docker container directly. To do so, first
shell into the Docker container:

    docker exec -it openagbraindockerrpi_brain_1 bash

Then, activate the [catkin workspace](http://wiki.ros.org/catkin/Tutorials/using_a_workspace)
within the Docker container:

    source catkin_ws/devel/setup.bash

Now, you can interact with ROS.

To list available ROS topics:

    rostopic list

To log output from a ROS topic:

    rostopic echo <topic name>

To learn more about ROS, check out the [ROS wiki](http://wiki.ros.org/).

## Re-flashing the Arduino firmware

openag_brain monitors the firmware configuration in the database and will
automatically re-flash the Arduino's firmware whenever it changes.

When doing firmware development, an easy way to force a re-flash of the
firmware is by bumping the revision number of anything in the
`firmware_module` database. Navigate to:

    http://<IP_ADDRESS>:5984/_utils/database.html?firmware_module

Select a record, hit save. The firmware flashing routine will kick
off automatically.

If you need to "re-start the world", you can stop and restart the openag_brain
docker container:

    docker restart openagbraindockerrpi_brain_1

**This will destroy the old database** and load configuration from the fixtures
stored in the openag_brain image.
