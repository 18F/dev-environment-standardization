---
permalink: /virtualization/docker/
title: Docker
parent: Virtualization
---


### Troubleshooting

#### Cannot connect to the Docker daemon. Is the docker daemon running on this host?

You need to bring your Docker environment variables into the scope of your current shell.

Get the name of the machine you want by running:

```sh
$ docker-machine ls
```

Once you have the machine name, run:

```sh
$ eval "$(docker-machine env <your machine name>)
```

#### "Network timed out while trying to connect to `https://index.docker.io/v1/repositories/library/<some_image_name>/images`. You may want to check your internet connection or if you are behind a proxy."

Most likely your docker host has got into a weird state. Run the following commands:

```sh
$ docker-machine restart $DOCKER_MACHINE_NAME
$ eval "$(docker-machine env $DOCKER_MACHINE_NAME)"
```
