---
title: "Setting up and configuring WSL2"
tags:
    - wsl2
    - windows linux
    - subsystem
    - wsl distro
    - win-kex
---

The Windows Subsystem for Linux (WSL) lets developers install a Linux distribution (such as Ubuntu, OpenSUSE, Kali, Debian, Arch Linux, etc) and use Linux applications, utilities, and Bash command-line tools directly on Windows, unmodified, without having to use a traditional virtual machine or dualboot setup.

#  Prerequisites

You must be running Windows 10 version 2004 and higher (Build 19041 and higher) or Windows 11 to use the simple commands for installing WSL.

# Choosing a method

There are two ways to run a Linux distribution using WSL. 

## Using Windows Terminal

A more traditional way, using the **Windows Terminal** to access the Bash command-line tools and other utilities directly on Windows 10 or 11. It is simple and lightweight, recommended if you are experienced with the Linux Terminal.
 
 [This is how the terminal version looks](https://i.imgur.com/X1tn84o.png)

## Graphical Desktop Environment

Using a **Graphical Desktop Environment** which brings the full Linux experience to our Windows operating system, utilizing protocols and clients **native to Windows** *(without additional software or virtual machine)*.
 
 [This is how the desktop environment looks](https://i.imgur.com/5HBjVl4.png)

# Installing WSL2

## Installing WSL without a graphical environment

Open a Windows Terminal and run the following command: <br>
`wsl --list --online` . This will show all the available Linux distributions. It is highly recommended to choose one from the distributions listed there.

After deciding, run the following command to install it: <br>
`wsl --install -d <Distro Name>` . Replace `<Distro Name>` with the exact name of the Linux distribution you would like to install.

Once you have installed WSL and the distribution, another terminal will pop up asking you to create a user account and password for your newly installed Linux distribution. Follow the steps shown on the screen.

After finishing, you are ready to use Linux on Windows. Try checking a few Bash commands like 
`ls`, `sudo apt update`, `cat /etc/os-release`.

*WSL version 2 will be installed by default. For more information, see: `wsl --help`.*

## Installing WSL with a graphical desktop environment
For this method, you will need a base distribution installed, preferably Kali Linux. To do this, run the following command: <br>
`wsl --install -d kali-linux` . After creating a user account and password, run the following commands in an opened Linux terminal: `sudo apt update`  and `sudo apt install -y kali-win-kex`.


### Optional: Installing the full package
To have the full Kali Linux experience, enter this command: `sudo apt install -y kali-linux-large` .This will install all the required tools, softwares and utility packages for programming, cybersecurity and much more. 

***Notice:*** *this may take longer to install and might use up to 20GB of space. Any required software can be manually installed later.*
 

*(To install the graphical environment for Ubuntu [please visit their site and follow the steps.](https://ubuntu.com/tutorials/install-ubuntu-on-wsl2-on-windows-11-with-gui-support#1-overview))*

# Launching WSL2

## Starting Linux in terminal mode
To launch your installed Linux distribution, start a Windows Terminal, click on the [arrow next to the new tab](https://i.imgur.com/aLcHvrC.png) button and from the dropdown list select Linux. You can also open it by searching your distribution [in windows search](https://i.imgur.com/iT4w9za.png).
You can start using it as soon as the theme changes to Bash terminal and you see your Linux username: 
**`name@PCname:~$`**

## Starting Linux in desktop mode
 
- To launch the desktop mode, run : `kex --esm -s` <br> (*ESM mode helps to keep the Windows and Kali environments visually apart,  -s is for enabling sounds)*. 
- Enter your linux password when asked.
- A [window](https://www.kali.org/docs/wsl/win-kex-esm/RDP-Message-1.png) will pop up asking to connect. Click **connect** to launch the desktop environment.

## Shutting down Linux 
Please note, that closing the Windows Terminal might not fully shut down your linux session! Check if it's still running by entering this command on a **Windows Terminal**: `wsl -l -v` .<br> To fully stop it, simply type `wsl --shutdown`. 
When exiting desktop mode, click the power button on the top right corner, then log out. You will be asked to save the session. By [ticking it](https://i.imgur.com/u8z1008.png), your changes will be saved on the next startup.

# Accessing Linux root directory

It is possible to access the installed distribution's directory directly in **File Explorer** on Windows. To do so, simply run this command in a Linux terminal: `explorer.exe .` . This allows you to access the root folder and create, delete, and easily modify any directories. Furthermore, you can transfer documents from your Windows PC to your local Linux folder.

# Uninstalling WSL
To uninstall WSL you only need to remove the previously installed distributions. To check what distros are installed, run: 
`wsl --list`. To remove any of them, simply use the following command: `wsl --unregister <distro>` . Replace `<distro>` with your Linux version's name. This will unregister the distribution and delete the root filesystem.


_Guide created by: @ChimiChumi, @mollevi._

_Source: [WSL installation](https://learn.microsoft.com/en-us/windows/wsl/install), [Win-Kex for WSL](https://www.kali.org/docs/wsl/win-kex/)_
