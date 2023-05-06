---
title: "Párhuzamosság a Java programozási nyelvben"
tags:
    - programming
    - Java
---

## Modellek a párhuzamosság kezelésére

Ebben a cikkben külön fogalmoként kezelem az egyidejűséget (Concurrency) és a párhuzamosságot (parallelism). Az előbbi inkább a lehetőséget biztosítja a feladatok egyidejű elvégzésére azzal, hogy hatékony kezelést nyújt a koordinációra és erőforrás-elérésre a feladatvégrehajtások között.
A párhuzamosság az erősforrások kihasználására alkalmazott módszer, mely során több processzor magon történik számítás. A feladatok felosztásával kezdődik és (ha kell), az eredmények kombinálásával végződik.

Az egyidejűség nehéz. 
Ide tartoznak olyan fogalmak, mint lock, semaphore, szinkronizáció.

A Java nyelv nagy előrelépéseket tett az évek alatt az ügyben, hogy az egyidejűséget könyebbé tegye. Modellek sokaságával rukkolt elő, mind különböző célra kifejlesztve. Ezek a következők:

- Blokkolás-gátló egyidejűség: Alapvető primitív a szál (Thread). Például fix számú fileok olvasására, hogy ne blokkoljon az IO művelet idejéig a program 
- Feladat-alapú egyidejűség: Thread pools, Szemafórok. Például webszerverek minden kérésre külön szálat indítanak 
- Adat alapú egyidejűség: fork-join library, melynek könyebb használatát később a parallel streams tette lehetőve
- ASYNC - AWAIT / PROMISE / TASK: Ezek a szavak szinte ugyan azt a fogalmat takarják. Az asyncron kódot beültetik egy olyan szintakszisba, mely a hagyományo syncron műveletekhez hasonlít. Gyakran használják a párhuzamosság elérésére, ugyanis könnyű elindítani több task-ot egyidőben. Ez a módszer is fork-joint használja az alacsonyabb rétegekben 

Mindegyik modellnek megvan a maga helye. Én a parallel streams-et mutattom be.

### Mi az a stream?

Egyszerűen fogalmazva: Egy stream olyan mint egy futószalag. A benne lévő elemeken elvégezhetők műveletek, de nem lehet őket elérni addig, amíg a műveletek nincsenek befejezve. 

Tehát egy stream-nek nincs `.next()` metódusa, mivel nincs koncepciója a sorrendiségről. Pont ez miatt lehet párhuzamosítani a műveleteket. Egy példa a nem párhuzamos használatukra:

```
widgets.stream()
              .filter(w -> w.getColor() == RED)
              .mapToInt(w -> w.getWeight())
              .sum();
```
Ez a kód egy kollekciónyi widgetet először megszűr a piros színre, majd veszi a weight property-jüket és int-é konvertálja őket, kapva egy int-eket tartalmazó stream-et. 
A végén ezeket az int-eket összegezzük.

 Ha szeretnénk például listaként kezelni őket, vissza kell konvertálnunk őket listává. 

```
widgets.stream()
    .filter(w -> w.getColor() == RED)
    .mapToInt(w -> w.getWeight())
    .sum()
    .toList().get(valahanyadik elem);
```

### Könnyű párhuzamosítás

Mint említettem, egy stream-ből nem nyerhetünk ki elemeket, így zárt rendszert alkotnak. A párhuzamosításuk triviális ( az api felhasználóiként) a **parallelStream** osztály segítségével!

```
widgets.parallelStream()
    .filter(w -> w.getColor() == RED)
    .mapToInt(w -> w.getWeight())
    .sum()
    .toList().get(valahanyadik elem);
```

Ez egyben szinkronizációs művelet is, mivel a toList csak akkor fog meghívódni, amikor már befejeződött a párhuzamos futás az egész stream kollekción. Így roppant könnyen elérhető az adatok párhuzamos kezelése.




