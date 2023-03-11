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

## Letöltés

Az Oracle VirtualBox letölthető az [eredeti oldalról](https://www.virtualbox.org/wiki/Downloads), több architektúrára és operációs rendszerre is. Példánkban Windows 10 és ArchLinux hostra is fogjuk telepíteni.

![Oracle VirtualBox letöltési opciói](screenshots/virtualbox_download.png)

Windows 10 hosthoz válasszuk az első lehetőséget, ArchLinux esetén használjuk a csomagkezelőnket a telepítéshez (ArchLinux esetén a letöltés és a telepítés egyetlen parancsból áll).
## Telepítés

### Telepítés Windows 10 gazdagépre

1. Keressük meg a letöltési helyen a letöltött telepítő fájlt.
2. Indítsuk el a telepítőt, és engedélyezzük a futását rendszergazdaként.
![UAC engedélykérés](screenshots/VirtualBox_Windows_10_INSTALL_0.png)
3. Az üdvözlő képernyőn kattintsunk a "Next" feliratú gombra.
![Üdvözlő képernyő](screenshots/VirtualBox_Windows_10_INSTALL_1.png)
4. A megjelenő képernyőn láthatjuk a telepítendő komponenseket, és a telepítési helyet. A modulokon nem érdemes változtatni, a telepítési helyet mindenki saját belátása szerint egy rögzített adattároló egységre (SSD vagy HDD) telepítse.
![Modul- és helyválasztó](screenshots/VirtualBox_Windows_10_INSTALL_2.png)
5. A megjelenő képernyőn a telepítő figyelmeztet, hogy a virtuális hálózati eszköz telepítése miatt időlegesen lecsatlakozunk az internetről. A kérdésre, miszerint "Kívánja-e folytatni?" igennel (Yes) válaszolunk.
![Hálózati kártya figyelmeztetés](screenshots/VirtualBox_Windows_10_INSTALL_3.png)
6. A képernyőn megjelenik egy utolsó lehetőség visszafordulni, és változtatásokat tenni (Back), vagy a telepítést megszakítani (Cancel). Mivel mi az előbbi lehetőségeink közül egyiket sem szeretnénk (tehát telepítésre készen állunk), az "Install" feliratú gombra kattintva jóváhagyjuk a telepítést.
![Telepítés jóváhagyása](screenshots/VirtualBox_Windows_10_INSTALL_4.png)
7. A telepítést a folyamatjelzőn és a folyamatjelző fölötti információs sorban nyomonkövethetjük.
8. A következő ablakban értesít a telepítő a sikeres (vagy sikertelen) telepítésről. Sikeres telepítés esetén a pipa kivétele nélkül kattintsunk a "Finish" feliratú gombra, amely ezen telepítőt bezárja, és az Oracle VirtualBox kezelőfelületét megnyitja.

Ha a jelölőnégyzet hamis értékre van állítva (nincs bejelölve pipával), a kezelőfelület nem fog elindulni a telepítő bezárását követően, hanem manuálisan (Start menüből, vagy asztali parancsikonnal) kell elindítanunk.
{: .notice--primary}

![Telepítés vége](screenshots/VirtualBox_Windows_10_INSTALL_5.png)

### Telepítés ArchLinux gazdagépre
1. Nyissunk egy terminált!
2. Root felhasználóként lépjünk be a linuxos gépünkbe.
3. Gépeljük be az alábbi parancsot: `pacman -Syu` (ezzel frissítjük csomagjainkat).
4. Nézzük meg milyen kernellel rendelkezünk, majd az alapján telepítsük fel az alábbi csomagokat.
    - `linux` kernel esetén: `pacman -S virtualbox virtualbox-host-modules-arch`
    - `linux-lts`, `linux-hardened`, és `linux-zen` esetén: `pacman -S virtualbox virtualbox-host-dkms`

Le tudjuk kérdezni az aktuális kernelt az `uname -r` parancs segítségével.
{: .notice--info}
5. Ha mindennel készen vagyunk, és nem kaptunk hibát, az alkalmazásmenüből a Rendszereszközök menüpont alatt elérhetjük "Oracle VM VirtualBox" néven.

## A VirtualBox beállítása

### Ajánlott kezdeti beállítások

1. Nyissuk meg az Oracle VirtualBox kezelőt!
2. A fájl (File) menüben kattintsunk a Beállítások (Preferences) menüpontra *vagy* nyomjuk meg a CTRL+G billentyű kombinációt a kezelőfelület kezdőképernyőjén.
3. Az Általános (General) oldalon érdemes a leendő virtuális gépek alapértelmezett mappáját (Default Machine Folder) átállítani olyan meghajtóra (vagy a meghajtó egy partíciójára), amely HDD eszközön van, mert az SSD-ket hamar tönkreteheti a folyamatos írás-olvasás.
![General lap](screenshots/VirtualBox_BEALLITAS_general.png)
4. Az Update oldalon érdemes bepipálni a lehetőséget, hogy időközönként keressen frissítéseket a VirtualBox.
![Update lap](screenshots/VirtualBox_BEALLITAS_update.png)
    - A Once per érték beállításával az frissítés keresésének a gyakoriságát állíthatjuk.
    - A Next check megjeleníti a Once per aktuális értéke alapján az első frissítés keresési időpontot.
    - A Check for rádiógombok használatával beállítható a frissítés típusa, amelyre keresünk. Ezt érdemes Stable Release Version értéken hagyni a stabil verziókhoz.
5. A Nyelv (Language) lapon elérhető a kezelőfelület nyelve. Az alapértelmezett (Default) érték az angol.

    A magyar fordítás helyenként pontatlan vagy hiányos lehet! Ha nem feltétlenül ragaszkodunk túlságosan a magyar nyelvű felülethez, nem érdemes módosítani.
    {: .notice--warning}

A beállítások között más opciókat is lehet állítani, mint például Proxy-t, vagy a megjelenítési témát. Ezek átállítása személyes preferencia (és szükség szerint) változtatható.

## Előkészületek a virtualizációhoz

### Virtuális médiák kezelése
Minden virtuális gépnek - éppúgy, mint a fizikai gépeknek - szüksége van valamilyen bootolható tárolóeszközre. Általánosságban a booteszköz lehet hordozható CD vagy DVD vagy USB eszköz (esetleg Floppy), illetve a rögzített SSD vagy HDD (pontosabban annak boot partíciója). Ezen eszközök létrehozhatóak a virtuális gép létrehozásakor, vagy önmagukban is. Ebben a részben az utóbbival fogunk foglalkozni.
1. Kattintsunk a File menü Tools almenüjében található Virtual Media Manager menüpontra *vagy* nyomjuk meg a `CTRL+D` kombinációt.
![Virtuális médiakezelő menüpontja](screenshots/VirtualBox_Mediakezelo.png)
2. A megjelenő ablaktáblában láthatjuk a virtuális eszközeinket (alapértelmezetten a virtuális merevlemezeket).
    - Az "Add" lehetőséggel már meglévő virtuális eszközt adhatunk hozzá (a lejjebb kiválasztott típusból) fájluk alapján.
    - A "Create" lehetőséggel létrehozható új virtuális eszköz (a lejjebb kiválasztott típusból).
![Virtuális médiakezelő lapja](screenshots/VirtualBox_Medialap.png)

### Virtuális merevlemez készítése
1. Kerüljünk a Virtual Media Manager ablaktáblára (lásd.: Virtuális médiák kezelése).
2. Válasszuk ki a "Hard disks" fület
3. Kattintsunk a "Create" gombra.
4. A megjelenő ablakban válasszunk egy lemeztípust, majd kattintsunk a "Next" feliratú gombra.
![Virtuális merevlemez típusa](screenshots/VirtualBox_CreateVHardDisk_0.png)
5. A következő ablakban meghatározhatjuk, hogy szeretnénk-e előre leallokálni az egész lemez méretét egyszerre, vagy hagyjuk dinamikusan növekedni a megadott limitig. Ha előre leallokáljuk a teljes méretet, a létrehozás hosszabb lesz, de teljesítményben gyorsabb is lesz. Ha beállítottuk preferenciánk szerint, kattintsunk a "Next" feliratú gombra.
![Allokáció típusa](screenshots/VirtualBox_CreateVHardDisk_1.png)
6. Ebben az ablakban meghatározható a virtuális merevlemez fájljának helye és mérete. Ha beállítottuk a lemezünk helyét és méretét, kattintsunk a "Finish" feliratú gombra.
    - Windows 10 Professional (x64) ajánlott lemezméret: minimum 30 GB
    - A GNU/Linux disztribúciókra jellemző lemezméret: általában 5 GB és 30 GB között
![A merevlemez mérete és mentési helye](screenshots/VirtualBox_CreateVHardDisk_2.png)
7. Az ablak bezárulta után látható, hogy a merevlemezünk megjelent a listán, és a műveleti lehetőségeink is kibővültek az ablaktábla felső részén.
![Ablaktábla az új virtuális merevlemezünkkel](screenshots/VirtualBox_CreateVHardDisk_3.png)

### Virtuális optikai lemez (CD vagy DVD) készítése ISO lemezképfájlból.

