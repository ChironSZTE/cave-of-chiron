---
title: "WSL 2 használata és beállítása"
tags:
    - Windows Subsystem for Linux
---

A Windows Subsystem for Linux (WSL) lehetővé teszi a fejlesztőknek, hogy egy Linux disztribúciót telepítsenek eszközükre (például Ubuntu, OpenSUSE, Kali, Debian, Arch Linux stb.), és közvetlenül a Windows rendszeren használják a Linux alkalmazásokat, eszközöket és a Bash parancssori utasításokat, anélkül hogy hagyományos virtuális gépet vagy dualboot beállítást kellene használni.


# Előfeltételek

A WSL egyszerű telepítéséhez szükséges, hogy a Windows 10 verziója legalább 2004 legyen (vagy újabb) és a Build száma legalább 19041 legyen (vagy újabb). A Windows 11 alapból támogatja a WSL-t.

# Módszer kiválasztása

Két fő módszer létezik a Linux disztribúció futtatására a WSL-en keresztül. Az alábbiakban választhatsz hogy melyik tetszik jobban.

## Windows terminálon keresztül

Ez egy hagyományosabb módszer amely a **Windows terminált** használja a Bash parancssori utasítások és eszközök elérésére, közvetlenül Windows 10 vagy 11 rendszeren. Ez egyszerű és gyors megoldás, a Linux terminálban jártasak számára ajánlott.

 [Így néz ki Windows terminálban](https://i.imgur.com/X1tn84o.png)

## Grafikus asztali környezet (Graphical Desktop Environment)
Egy grafikus asztali környezet (Graphical Desktop Environment) segítségével, amely teljes Linux élményt nyújt a Windows operációs rendszerben, kihasználva a Windows-nak a protokollokhoz és kliensekhez való natív hozzáférést (**további szoftverek vagy virtuális gépek nélkül**).
 
 [Így néz ki a grafikus környezet](https://i.imgur.com/5HBjVl4.png)

# WSL2 telepítése

## Telepítés grafikus környezet nélkül

Nyiss meg egy Windows Terminált és futtasd az alábbi parancsot: `wsl --list --online` . Ez ki fogja listázni az összes elérhető Linux disztribúciót. Nagyon ajánlott az ott felsorolt disztribúciók közül választani egyet.

Miután kiválasztottad, futtasd a következő parancsot a telepítéshez: <br>
`wsl --install -d <Distro Name>` . Írd át a `<Distro Name>` részt a pontos Linux disztribúció nevével, amelyet telepíteni szeretnél.

Amint telepítetted a WSL-t és a disztribúciót, egy másik terminál fog megjelenni ami kérni fog hogy hozz létre egy linux felhasználónevet és adminisztrátor jelszót az újonnan telepített Linux disztribúció számára. Kövesd az ablakban megjelenő lépéseket.

Ha sikerült, már készen is állsz arra, hogy Linuxot használj Windows-on. Próbálj ki néhány Bash parancsot: <br>
`ls`, `sudo apt update`, `cat /etc/os-release`.

*Alapértelmezetten a WSL 2-es verziója fog telepítődni. További információkért, lásd: `wsl --help`.*

## WSL telepítése grafikus felhasználói felülettel
Ehhez a módszerhez szükséged lesz egy alap Linux disztribúcióra, lehetőleg **Kali Linux**-ra. Ehhez futtasd a következő parancsot: `wsl --install -d kali-linux` . Miután létrehoztál egy felhasználói fiókot és jelszót, futtasd a következő parancsokat egy megnyitott Linux terminálban: `sudo apt update` és `sudo apt install -y kali-win-kex`.


### Opcionális: Teljes csomag telepítése
Ha a teljes Kali Linux csomagot szeretnéd telepíteni, megteheted az alábbi parancsal:
`sudo apt install -y kali-linux-large`. Ezzel a parancsal minden szükséges eszközt, szoftvert és segédprogramot telepíteni fog programozáshoz, kiberbiztonsághoz és egyéb feladatokhoz.

***Figyelem!*** *Ez a telepítés hosszabb ideig eltarthat, és akár 15 GB helyet is igénybe vehet. Minden egyéb szoftvert manuálisan is telepíteni tudsz késöbb.*
 

*(Az Ubuntu grafikus környezetének telepítéséhez [kérlek látogass el a hivatalos oldalukra ahol további útmutatást kaphatsz.](https://ubuntu.com/tutorials/install-ubuntu-on-wsl2-on-windows-11-with-gui-support#1-overview))*

# WSL2 futtatása

## Linux indítása terminál módban
A Linux disztribúció indításához nyiss meg egy Windows terminált, kattints a [új lap melletti nyilra](https://i.imgur.com/aLcHvrC.png) és a lenyíló menüben válaszd ki a Linuxot. Ugyanakkor megnyithatod 
[rákeresve a Windows alkalmazások között](https://i.imgur.com/iT4w9za.png).
Amint a konzol átvált Bash terminálra és megjelenik a Linux felhasználóneved, már használhatod is:
**`name@PCname:~$`**

## Linux indítása asztali üzemmódban
 
- Az asztali üzemmód elindításához futtasd a következő parancsot: `kex --esm -s` <br> (*Az ESM mód vizuálisan elkülöníti egymástól a Windows és a Kali környezetet, a -s pedig a hangok engedélyezésére szolgál.)*. 
- Gépeld be a linux sudo jelszavad amikor kéri
- Egy [ablak](https://www.kali.org/docs/wsl/win-kex-esm/RDP-Message-1.png) fog megjelenni amely felkér a csatlakozásra. Klikkelj a **connect** gombra hogy elindítsd a desktop környezetet.

<br>
*További információkért, lásd: `kex --help`*
## Linux leállítása
- Vedd figyelembe, hogy a Windows terminál bezárása nem állítja le teljesen a Linux munkamenetet! Ellenőrizd, hogy még fut-e a WSL a következő parancs segítségével a **Windows terminálban**: 
`wsl -l -v` . A teljes leállításhoz, használd a `wsl --shutdown` utasítást. Ez hasznos lehet abban az esetben ha eszközöd lassabb.
- Az asztali üzemmódból való kilépéshez kattints a jobb felső sarokban lévő power gombra, majd jelentkezz ki. Lehetőséged van elmenteni a munkamenetet. Ehhez [pipáld ezt be](https://i.imgur.com/u8z1008.png), hogy változtatásaid mentve maradjanak a következő indításnál.

# Linux gyökérkönyvtárának elérése

Lehetőséged van a telepített disztribúció könyvtárának közvetlen elérésére a File Explorer segítségével Windows-on. Ehhez futtasd a következő parancsot a Linux terminálban: `explorer.exe .` . 

Ez megnyitja a gyökérkönyvtárat, és megfelelő jogok mellett képes vagy új mappákat létrehozni, törölni vagy akár módosítani. Emellett egyéb dokumentumokat és fájlokat is könnyedén át tudsz helyezni Windows PC-ről a helyi Linux könyvtáradba.

# WSL eltávolítása
Az WSL eltávolításához csak az előzőleg telepített disztribúciókat kell eltávolítanod. Hogy megnézd milyen disztribúciók vannak telepítve, futtasd a következő parancsot:
`wsl --list`. A törléshez használd a következő utasítást: <br> `wsl --unregister <distro>`, ahol a `<distro>` a törölni kívánt Linux neve. Ez törölni fogja a disztribúciót és az összes kapcsólódó fájlt a gépedről.


_Az útmutatót készítette: Dobos Hunor (@ChimiChumi) és Molnár Levente (@mollevi)_

_Forrás: [WSL installation](https://learn.microsoft.com/en-us/windows/wsl/install), [Win-Kex for WSL](https://www.kali.org/docs/wsl/win-kex/)_
