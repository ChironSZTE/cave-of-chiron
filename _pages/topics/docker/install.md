---
title: Installation
tag: Docker
---

Docker has 2 editions you can choose from when installing:
- [Docker Engine](https://docs.docker.com/engine/): The main part of Docker, manageable from the command line (recommended)
- [Docker Desktop](https://docs.docker.com/desktop/): A GUI app for managing your Docker environment. Includes Docker Engine. It always runs the containers in a shared virtual machine.

Docker Engine is recommended mainly because it is easier to use, _in my opinion_.  
If you are a student at SZTE, and you are doing the "Rendszerfejlesztés 2" course in the spring semester of 2023, this is also what I can help with on [the course forum](https://www.coosp.etr.u-szeged.hu/Scene-718272/Forum-2760968).  
If you use Docker Desktop, feel free to ask still, I (or someone else) may still be able to help.
A lot of things are the same, but a fundamental change in how it works might make things a bit more difficult to manage.  
Last but not least, Docker Engine is open source, while the additions of Docker Desktop are not.

Docker Desktop is expected to also have higher resource usage, because of the use of a virtual machine.

## Linux

On Linux based systems, you most probably want to install Docker from the package manager of your distribution, so that its packages get updated along with everything else it depends on.  
This means the method depends on which distribution you use.

Generally, it is best to read the official installation instructions ([Engine](https://docs.docker.com/engine/install/#server), [Desktop](https://docs.docker.com/desktop/install/linux-install/)), which also covers the system requirements.
For Docker Engine, start from the "Server" heading, as the "Desktop" heading redirects you to Docker Desktop.

## Windows

On Windows, installation is done by installing [Docker Desktop](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe).  
Docker Engine in itself is not available.

It is recommended to read the [official installation instructions](https://docs.docker.com/desktop/install/windows-install/), which also covers the system requirements, but the above installer should handle everything by itself.

At installation, you will need to choose between using the WSL2 or the Hyper-V backend. [You can switch](https://docs.docker.com/desktop/faqs/windowsfaqs/#how-do-i-switch-between-windows-and-linux-containers) later on.  
You may need to enable the WSL or Hyper-V features manually. If you still have access to the Control Panel, the steps to do so are described [here](docker-windows-enabled-features.md).

## Mac

On Mac, installation is done by installing Docker Desktop ([x86]([](https://desktop.docker.com/mac/main/amd64/Docker.dmg)), [ARM (M1)](https://desktop.docker.com/mac/main/arm64/Docker.dmg)).  
Docker Engine is not available in itself.

It is recommended to read the [official installation instructions](https://docs.docker.com/desktop/install/mac-install/), which also covers the system requirements, but the above installer should handle everything by itself.