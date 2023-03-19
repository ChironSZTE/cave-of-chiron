# Docker Containers

Containers facilitate running your software isolated from your Host system, and also from other containers.  
They also store a reference to the container image from which they were instantiated,
and all of the files and container configuration that were changed compared to the image.

## Basic Usage

### Running a new container

Likely the most frequent task you will do with containers is starting them from an image file.  
You do that with the `docker run [options] <image>` command.  
If the image to be used have not been downloaded beforehand, the command will automatically do so.

This **always** creates a **new** container from the specified image file, and start a predefined program in it. The program to be started, its arguments and other configuration can be overridden with options.

When the `--detached` option is not used, the container will run in the terminal where it was started, until it exits by itself or is requested to do so.
In this case it can be stopped by pressing `Ctrl+C`. If you press it twice, Docker will not wait for the container to gracefully stop, instead it will kill it immediately, which might result in data corruption.

Be aware that by default containers are not deleted when stopped.  
That means, if you always create a new container this way, it might slowly fill up your storage space.

Below are some useful options. You don't need to memorize them, you can always come back when you need one, but it is recommended to at least skim through what is available.

|Option|Meaning|  
|---|---|
|-d, --detach|Run in background, without showing container logs and keeping the terminal busy|
|--name|Assign a name for easier identification. Otherwise the container will get a randomly generated human readable name|
|--rm|Delete the container on exit. Useful for temproary containers|
|--network|Attach the container to a preexisting Docker network by its name|
|-p, --publish|Bind networks ports of the container to the Host's network interfaces (all of them), so that they are accessible from the networks where the Host is accessible|
|--volume|Attach the specified Docker storage volumes to this container|
|--restart|Whether to automatically restart the container at certain conditions (shutdown, error, system reboot, etc)|
|-e --env|Run with the given environment variables|
|-i, --interactive|Keep STDIN stream open, so that the software running in the container can be controlled through it. Frequently used with `-t` for making it accessible from a terminal|
|-t, --tty|Allocate a virtual terminal. Frequently used with `-i`|
|--help|Lists all available options, including those that were not listed here|

### Listing existing containers

If you have already created containers, you may list them with the `docker container ls --all` command. The `--all` option makes sure that containers that are currently not running are also listed.

```
$ sudo docker container ls --all
[sudo] password for apophis:
CONTAINER ID   IMAGE                                 COMMAND                  CREATED       STATUS                   PORTS                                                                      NAMES
81af1462f96d   local/gitea/gitea:1.18.3              "/usr/bin/dumb-init …"   6 days ago    Up 6 days                2222/tcp, 3000/tcp                                                         gitea
e407f84e1272   ghcr.io/linuxserver/mariadb:10.6.12   "/init"                  6 days ago    Up 6 days                3306/tcp                                                                   gitea_db_1
1f7a0e4bb6ce   ghcr.io/linuxserver/mariadb:10.6.12   "/init"                  6 days ago    Up 6 days                3306/tcp                                                                   vikunja_db_1
d8a4f03849d4   ghcr.io/linuxserver/wireguard         "/init"                  2 weeks ago   Up 12 days               192.168.56.2:51820->51820/udp                                              wireguard
499a616c9e9a   traefik:v2.4                          "/entrypoint.sh trae…"   3 weeks ago   Up 12 days               0.0.0.0:80->80/tcp, :::80->80/tcp, 0.0.0.0:443->443/tcp, :::443->443/tcp   traefik
1e1617582ae8   pihole/pihole:2022.10                 "/s6-init"               3 weeks ago   Up 12 days (healthy)     192.168.56.2:53->53/tcp, 192.168.56.2:53->53/udp, 67/udp, 80/tcp           pihole
[...]
```

There are 2 ways for identifying containers in docker commands:
- by their container ID (e.g. `81af1462f96d`)
- by their name (e.g. `gitea`)

It is common for container names to indicate the software they are running.

### Starting and stopping an existing container

If you have already created a container, and you want to reuse it, you can start it with the `docker container start <container>` command, where you have to provide the name or ID of the container to be started.

You can stop a running container with the `docker container stop <container>` command.  
When stopping a container, its filesystem and configuration is kept.

Containers are attached to the images from which they were instantiated.
If the image file to be used has changed (a new version was released), you will have to create a new container from the new image file to use it.

### Deleting containers

If you don't need a container anymore, you can delete it with the `docker container rm [options] <container>` command.  
For deleting a running container, you will need to use the `--force` option.  
For deleting all anonymous volumes of a container, you will need to use the `--volumes` option. This is recommended to use when you really don't need the container anymore.

Deletions are not reversible, take extreme care when deleting containers or volumes.

### The container upgrade procedure

When a new version of the containerized software is released, updating it differs from how is it done usually.  
Instead of updating the software in the container with the package manager or with manually overwriting files, you just download the new container image, delete the old container, and recreate it with the same (or adjusted if necessary) settings as before.

As all important data is stored on volumes, nothing of value should be lost.

## Advanced usage

All of the container management commands are subcommands of the `docker container` command.  
They can be listed by running `docker container --help`, but below I have made a list of the more useful ones:

|Subcommand|Meaning|
|---|---|
|start|Starts an existing container. For details see above|
|stop|Stops a running container. For details see above|
|restart|Restarts a running container in one step|
|ls|Lists running/existing containers. For details see above|
|exec|Execute a command in a running container. Commonly used with the `-it` options|
|logs|Read logs printed by the main process of the container|
|rename|Rename the container|
|commit|Create a new container image from the current state of the container|

All of the subcommands will print their detailed usage and available options when run with the `--help` option.
All of them are also documented in detail [here](https://docs.docker.com/engine/reference/commandline/container/).