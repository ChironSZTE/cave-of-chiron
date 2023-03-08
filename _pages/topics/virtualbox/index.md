---
title: "Oracle VirtualBox telepítése, konfigurálása, és használata"
tags:
    - Oracle
    - VirutalBox
    - virtualization
    - VM
    - virtual machine
    - linux
    - windows
    - BSD
---

## Bevezetés

Az [Oracle VirtualBox](https://www.virtualbox.org/) egy virtualizációs termék. Segítségével virtuális számítógépeket hozhatunk létre tetszőleges (de a fizikai erőforrásainkat nem meghaladó) erőforrásokkal, majd ezen virtuális gépeken - éppúgy, mintha fizikai lenne - telepíthetünk operációs rendszereket, majd ezekre a rendszerekre felhasználói programokat. A Dockerrel ellentétben itt egy egész számítógépet virtualizálunk, nem csak a felhasználói (vagy operációs rendszer) programokat.

### Letöltés

Az Oracle VirtualBox letölthető az [eredeti oldalról](https://www.virtualbox.org/wiki/Downloads), több architektúrára és operációs rendszerre is. Példánkban Windows 10 és ArchLinux hostra is fogjuk telepíteni.

![Oracle VirtualBox letöltési opciói](screenshots/virtualbox_download.png)

Windows 10 hosthoz válasszuk az első lehetőséget, ArchLinux esetén használjuk a csomagkezelőnket a telepítéshez (ArchLinux esetén a letöltés és a telepítés egyetlen parancsból áll).
### Telepítés

#### Telepítés Windows 10 gazdagépre

#### Telepítés ArchLinux gazdagépre
Nyissunk egy terminált, és gépeljük be az alábbi parancsot: