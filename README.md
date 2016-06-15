openag\_brain\_docker\_rpi
-----------------------
This repository provides a docker-compose configuration for running the
[openag\_brain](http://github.com/OpenAgInitiative/openag_brain) project on a
Raspberry Pi.

To run the project, first clone this repository:

    git clone http://github.com/OpenAgInitiative/openag_brain_docker_rpi
    cd openag_brain_docker_rpi

Now, install `docker` and `docker-compose`. This repository has a script that
does this for you. It leverages a repository set up by
[Hypriot](http://blog.hypriot.com/) which provides the necessary packages.

    sh install_docker.sh

Now, the project can be run as follows:

    docker-compose up -d
