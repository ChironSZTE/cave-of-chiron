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

Docker has 2 editions you can choose from when installing:
- [Docker Engine](https://docs.docker.com/engine/): The main part of Docker, manageable from the command line (recommended)
- [Docker Desktop](https://docs.docker.com/desktop/): A GUI app for managing your Docker environment. Includes Docker Engine. It always runs the containers in a shared virtual machine.

Docker Engine is recommended mainly because it is easier to use, _in my opinion_.  
If you are a student at SZTE, and you are doing the "Rendszerfejlesztés 2" course in the spring semester of 2023, this is also what I can help with on [the course forum](https://www.coosp.etr.u-szeged.hu/Scene-718272/Forum-2760968).  
If you use Docker Desktop, feel free to ask still, I (or someone else) may still be able to help, but I'm not familiar with it, and because of the additional virtualization layer there might be difficulties. A lot of things are similar, but a lot of things aren't.
Last but not least, Docker Engine is open source, while the additions of Docker Desktop are not.

Docker Desktop is expected to also have higher resource usage, because of the use of a virtual machine.

### Linux

On Linux based systems, you most probably want to install Docker from the package manager of your distribution, so that it gets updated along with everything else it depends on.  
This means the method depends on which distribution you use.

Generally, it is best to read the official installation instructions ([Engine](https://docs.docker.com/engine/install/#server), [Desktop](https://docs.docker.com/desktop/install/linux-install/)), which also covers the system requirements.
For Docker Engine, start from the "Server" heading, as the "Desktop" heading redirects you to Docker Desktop.

### Windows

On Windows, installation is done by installing [Docker Desktop](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe).  
Docker Engine is not available.

It is recommended to read the [official installation instructions](https://docs.docker.com/desktop/install/windows-install/), which also covers the system requirements, but the above installer should handle everything by itself.  
However it is important to note, that current versions (2023Q1) require at least Windows 10 21H1 (20H2 if Enterprise or Education edition).

At installation, you will need to choose between using the WSL2 or the Hyper-V backend. [You can switch](https://docs.docker.com/desktop/faqs/windowsfaqs/#how-do-i-switch-between-windows-and-linux-containers) later on.  
You may need to enable the WSL or Hyper-V features manually. If you still have the Control Panel, the steps to do so are described [here](docker-windows-enabled-features.md).

### Mac

On Mac, installation is done by installing Docker Desktop ([x86]([](https://desktop.docker.com/mac/main/amd64/Docker.dmg)), [ARM (M1)](https://desktop.docker.com/mac/main/arm64/Docker.dmg)).  
Docker Engine is not available.

It is recommended to read the [official installation instructions](https://docs.docker.com/desktop/install/mac-install/), which also covers the system requirements, but the above installer should handle everything by itself.

## Configuration