# Using and configuring Docker

In this document I'll briefly introduce Docker, and the common operations of it you may need to use.

If you are a student at SZTE, and doing the "Rendszerfejlesztés 2" course in the spring semester of 2023, you are welcome at [the course forum](https://www.coosp.etr.u-szeged.hu/Scene-718272/Forum-2760968) if you need help, or if you have feedback.  
If you have feedback, I believe you can also open an issue at the [Cave of Chrion](https://github.com/ChironSZTE/cave-of-chiron) Github repository, tag it as a bug, and mention mpeter50.

## Introduction

Docker is a popular **containerization platform**.  
The point of containerization platforms is that as a developer/maintainer, you can package your software for a system with all the dependencies that it needs to properly run, instead of relying on the user to find the _correct_ versions of the _correct_ packages. It is usually also easier for the user. You will see why.

With Docker, any software you run is packaged into a "container".  
A container is like a regularly installed operating system, but most usually one based on Linux, and without a graphical environment. However it's important to note that these are not limitations, but how things are usually done, for practical reasons.

If you have used virtual machines, the concept might seem familiar, but there are a few important differences:
- when you tear down (~delete) a container, its state (mainly files) is forgotten, **by default**.
- the running containers all share the same kernel, usually of the Host OS (the OS on which docker runs).  
    The difference is in the "userspace": the software that is installed, and the files, like the configuration files.
    This means lower memory usage, and easier resource sharing (filesystems, network, ...), but also sharing of kernel **vulnerabilities**. Keep your Host OS (and Docker) up to date if you run containerized services published to any public network! (e.g. internet, school network)

Containers are usually distributed as container images.  
If someone has made one for the software you want to use, you can download and use it, but you can also make one yourself. It's quite easy, you'll see. 

## Installation

You can find information about installating Docker [on this page](docker/install.md).

## Usage

When using Docker, frequently you will deal with containers, and the resources used by them.  
When you use Docker Engine (but should apply to Docker Desktop too), you'll be able to manage these through the `docker` command and its subcommands.

When giving examples to commands, I may use `[foo]` and `<bar>`. These mean that `foo` is an optional parameter, but `bar` is mandatory.

Almost all docker commands have options for modifying their behavior,
but for the sake of simplicity, I may not list every one of them.
The full list of subcommands and options can be obtained with using the `--help` option for any command.

You may have to run the `docker` command with administrator privileges.

### Containers

Containers are the objects that run your software, and store its mutable state.  
You can read about them [on this page](docker/containers.md).

### Images

Images are the objects that store the immutable parts of your containers. It is not uncommon that images, or at least a part of them are shared between multiple containers.
You can read about them on this [page](docker/images.md).

### Networks

Containers are normally totally isolated from your Host system, but you can give them access to resources of it.  
You can use Docker Networks to define what kind of networks they have access to, be it the Host's LAN (with even internet access through it) or a small network only accessible by a select few containers.  
You can read about them on this [page](docker/networks.md).

### Volumes

Similarly to Docker Networks, volumes give access to filesystems of the Host system, or possibly only parts of it.  
Volumes are also commonly used for storing data that should not be forgot when replacing your container with a newer version.
You can read about them on this [page](docker/volumes.md).

## Configuration

Dockers configuration can be divided into 3 pieces:
- configuration of individual docker resources
- docker daemon configuration
- docker CLI configuration

The configuration of individual docker resources were covered in pages referenced from the usage section.

### Docker daemon configuration

The Docker daemon is the background process that controls the containers and all other Docker resources.  
It is usually [automatically started](https://docs.docker.com/config/daemon/start/) by the system, and runs until it is shut down.

The primary way of its configuration is by editing the `daemon.json` file.  
On Linux systems, it is located at `/etc/docker/daemon.json`.  
On Windows systems, it is located at `C:\ProgramData\docker\config\daemon.json`.

The available configuration options are documented [here](https://docs.docker.com/engine/reference/commandline/dockerd/).

There are 2 notable configuration options, both of which override default configuration values of containers:

|Option|Meaning|
|---|---|
|dns|Use a different set of DNS servers by default for every container. Useful if you run a DNS server on the Host, possibly as a Docker container, and you want Docker to be able to make use of locally defined domain names.|
|log-driver|Use a different log storage driver by default for every container. The [default `json-file` driver](https://docs.docker.com/config/containers/logging/json-file/) might be inefficient (large files) for chatty containers, compared to the [`local` driver](https://docs.docker.com/config/containers/logging/local/).|

### Docker CLI configuration

Docker CLI is the name of the `docker` command.

#### Contexts

[Contexts](https://docs.docker.com/engine/context/working-with-contexts/) allow Docker CLI to be used to control a different Docker Daemon than the default one.  
This is useful if you [run multiple Docker Daemons](https://docs.docker.com/engine/reference/commandline/dockerd/#run-multiple-daemons),
when you run it in [rootless mode](https://docs.docker.com/engine/security/rootless/),
or if you want to [control a remote Daemon](https://docs.docker.com/config/daemon/remote-access/) without the use of SSH.

## Advanced Usage

note on privileges: you dont have to be root. docker contexts.

note on sections:
- instead of talking about containers and volumes twice (first in basic usage, then in advanced usage), I could
  - break the sections up into pages
  - link to them from here
  - have a short description of the component here
  - include both the basic and advanced usage on the component dedicated page
- move example run excerpts to a new section after basic usage and advanced usage
- basic usage: basic, simpler command syntax; advacned usage: full command syntax