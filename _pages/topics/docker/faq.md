---
title: Frequently Asked Questions
sidebar:
    nav: "docker"
---

## How can I recognize the name of a container image?

Your teammates have said you need to run a container. You remember its name, but you are not sure about how was it written exactly.    

Container image names tipically look like one of these:
- `gitea/gitea`
- `gitea/gitea:1.19`

These are also commonly found on websites of Container Registries, like Docker Hub, but also in your software's (e.g. Gitea) own documentation.

## I have the name of a container image, how can I run it?

Your team has said you need to start up a new container thats called this way: `gitea/gitea:1.19`.   
You can run it with the following command:
```
sudo docker run gitea/gitea:1.19
```

The container will start up, and it will hold your command prompt until it has finished running.  
If you want to stop it, you can press `Ctrl + C`. You may need to wait a couple of seconds until it gracefully stops.

This command will assume a lot of default settings. If you need to customize anything, you can use options. You can read more about them in the [Containers](containers.md) tutorial.  
But it is worth mentioning, that it is common to use the `-d`  option, which makes the container run in the background.

## I have a Docker Compose file, how do I start it?

For more complex software it is common to use Docker Compose files,
which essentially contain everything (configurationwise) about how should the container be run, and what other things are needed for it to function properly.  
These files are commonly distributed along with the source code of the software.

Docker Compose files are called either of these ways:
- docker-compose.yml
- docker-compose.yaml

When you see one, you can make use of it by changing to the directory in your terminal where this file is located, and then by running the following command:
```
sudo docker compose up
```

The command will automatically find the file with the above name, and create all Docker resources and start the containers as specified inside of it.

It is worth mentioning, that similarly to the `docker run` command, it is common to use the `-d`  option, which makes the stack run in the background.

## I have a `Dockerfile`, how do I build the container using it?

Files whose name is either `Dockerfile`, or start with `Dockerfile.`, contain the instructions to make a new container image.  
These files are commonly distributed along with the source code of the software.

First you need to install the Docker BuildKit. You can find more information about that [here](https://docs.docker.com/go/buildx/).

Then you can start the build process with the following command:
```
sudo docker buildx build
```

Docker also has a `docker build` subcommand. Do not use that one, as it is deprecated, and it may not be able to build your container in the way you expect it.  
{: .notice--warning}

The Docker build system will look at the file named `Dockerfile`, and execute its instructions.  
Files whose name start with `Dockerfile.` are not used automatically, but you can make use of them by specifiying their name with the `--file` option.

When the build process has successfully finished, the resulting container image will be available for use by other Docker commands.
