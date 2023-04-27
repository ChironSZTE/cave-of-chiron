---
title: "VMware"
tags: vmware
sidebar:
nav: "vmware"

---
## Mappa megosztása VM és Host között:


Kezdésnek le kell állítani a virtuális gépet. Ez után van lehetőség szerkeszteni a beállításait.


-	A kiválasztott VM-nél meg kell nyitni a **Virtual Machine Settings** ablakot. (Ctrl + D)
-	Majd át kell lépni az **Options** fülre.
-	Itt a **Shared Folders** sort kell választani.
-	Jobb oldalon engedélyezni kell a megosztást **Always enable** opcióval
-	Be kell pipálni a **Map as a network drive in Windows guest**
-	Majd **Add…** gomb.![1.png](/images/1.png)
-	Itt nyílik egy varázsló:![2.png](/images/2.png)
- **Next**
- **Browse** gomb segítségével ki kell választani egy mappát a HOST-on amit meg szeretnénk osztani a VM-mel. (Lehet mappa is, de akár egy teljes meghajtó is)
- Majd adni kell neki egy nevet![3.png](/images/3.png)
- **Next**
- Itt lehetőség van pár beállításra, de alapértelmezetten megfelelő
- végül **Finish**![img_2.png](/images/4.png)
-	**OK** gombbal mentjük a beállítást![5.png](/images/5.png)
-	**Indulhat a VM**
-	Elindulás után **Windows Intézőben** (Win + E) láthatjuk is a mappát, mint egy hálózati meghajtót.
![6.png](/images/6.png)


