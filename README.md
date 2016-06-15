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

After installing `docker`, you may have to start a new terminal session in
order to be able to contact the docker daemon

Now, the project can be started as follows:

    docker-compose up -d

2 Docker containers will be started in the background and will persist across
reboots.
