---
title: "Markdown nyelv és használata GitLabon"
tags:
   - markdown
---

## Bevezetés
Miért a Markdown? Ez egy nagyon jó kérdés. Egy nagyon hasznos, és könnyen használható, illetve kevés helyet felhasználó nyelv, amit **John Gruber** talált fel 2004-ben. A nyelv haszna főleg a szövegformázásra, nagyon hasznos szintaxisokkal ellátott beágyazásokkal volt ellátva, dokumentálásra és tesztelésekre tökéletes volt az alkalmazása, mind a mai napig.

**FONTOS: A GitHub és GitLab nagyon hasonló `.md` nyelvet használ, ezért nem lesz különbontás, hanem egyben fogok őket venni.**


***A következő pár bekezdés során végigmegyünk a Markdown legfontosabb elemein, szintaxist és példákon keresztül megismerjük a nyelvet. A fő fókusz az alapokon van, de van sok más felhasználása is, ami most nem kerül bemutatásra.***
<br>

Csapjunk is bele!
## Fejlécek
Ahhoz, hogy készítsünk egy fejlécet, egy `#` karakternek kell állnia a szöveg előtt, egy szóközzel követve. A `#` száma fogja meghatározni, hogy mekkora lesz a fejlécünk.
```
# Ez ugyanaz, mint a <h1> elem a HTML-ben
## Ez ugyanaz, mint a <h2> elem a HTML-ben
### Ez ugyanaz, mint a <h3> elem a HTML-ben
```
stb.

## Szövegformázás
Sokfajta szövegformázás létezik, például vastag, dőlt, áthúzott, alsó vagy felsőindexű.

- Vastag -> `** **` or `__ __` -> **Ez egy pelda vastag szovegre**
- Dőlt -> `* *` or `_ _` -> *Ez egy pelda dolt szovegre*
- Áthúzott -> `~~ ~~` -> ~~Ez egy pelda athuzott szovegre~~
- Vastag és dőlt -> `*** ***` -> ***Ez egy pelda vastag es dolt szovegre***
- Alsóindex -> `<sub></sub>` -> Ez egy pelda<sub>formula</sub> szoveg
- Felsőindex -> `<sup></sup>` -> Ez egy pelda<sup>formula</sup> szoveg

## Szövegkiemelés, idézés
Idézheted is a szöveged, ami azt jelenti, hogy kap egy szürke vonalat a container elé, illetve szürke lesz maga a szöveg is.
> Ez egy pelda idezett szoveg

Használhatjuk szövegen belül, az ún. 'tompa ékezet' használatával.
Ez egy nagyon jo peldaszoveg `koddal` a vegen.

Ezzel a tompa ékezettel tudunk kódblokkokat is létrehozni, egymás utána 3-at is kell a blokk elé és végére tenni. Ha esetleg értelmes kódot is tartalmaz, akkor az első 3 tompa ékezet után a nyelvet is megadhatjuk, ekkor kiszinezi nekünk a kulcsszavakat automatikusan. Erre két példa is lesz, hogy lássuk a különbséget.

```
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello world!");
    }
}
```
<hr>

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello world!");
    }
}
```

Ahogy láthatjuk, a második kódrészlet sokkal jobban látszódik, mint az első, nagyon ajánlatos ezt használni.

## Linkek
Létrehozhatunk linkeket egyedi névvel is. A beállítani kívánt név a szögletes zárójelek közé `[]`, a link pedig rögtön utána, zárójelek `()` közé kerül.

`Ez egy pelda link a GitLabhoz: [Gitlab](https://about.gitlab.com/)`
[GitLab](https://about.gitlab.com/)

Ha szükséges, használhatunk **relatív útvonalat** is, a `/`, `./` és `../` használatával. Helyi fájloknál érdemes használni, kevesebb a hibalehetőség.

## Képek
Képeket is beszúrhatúnk kommentbe hasonló módon, mint ahogy linket is. A különbség az, hogy a szintaxis elé egy `!`-et kell rakni, és úgy már képként fogja érzékelni.

`![Ez egy pelda kep a GitLab ikonjarol](https://gitlab.com/uploads/-/system/group/avatar/6543/logo-extra-whitespace.png)`
![Ez egy pelda kep a GitLab ikonjarol](https://gitlab.com/uploads/-/system/group/avatar/6543/logo-extra-whitespace.png)

## Listák

Készíthetünk rendezetlen listát a `-`, `*`, vagy `+` használatával. Ha rendezett listát szeretnénk, a számjegy után egy pont kell, és el kell választani őket, listás sorrendben.

- Ez
+ Egy
* Nagyon
+ Rendezetlen
- Lista
<hr>

1. Ez
2. Egy
3. Nagyon
4. Rendezett
5. Lista

Egymásba is rakhatunk mindenféle listákat.

1. Első elem
    - Második elem
        - Harmadik elem

## Feladatlisták
Készíthetünk jelölőnégyzeteket, hogy tudjuk a feladatokat követni, egy `-`, szóköz és `[ ]` segítéségvel. A szóköz fontos a szögletes zárójelek közt, ez jelzi azt, hogy üres. Ha szeretnénk beikszelni, akkor a szóközt kell kicserélni egy `X`-re, így: `[X]`.

- [ ] #1111
- [ ] Első feladat
- [X] Második feladat

## Megemlítés
Kétfajta megemlítést különböztetünk meg:
1. Személyes és/vagy csapatok -> A `@` használatával -> `@<person/team>` -> @Nicewithothers
2. Issue-k és pull request-ek -> A `#` használatával -> `#<issue/pr_number>` -> #12

## Kommentek
Kommentelhetsz ezzel a szintaxissal: `<!-- Ez egy pelda komment szoveg -->`

## Végső gondolatok
Fontos megjegyezni, hogy lehet HTML elemeket is használni a Markdown nyelven belül. Én is sokat használtam. Illetve ez egy nagyon alap bemutatása ennek a nyelvnek, viszont ez bőven elég tudás, hogy tudjunk dolgozni vele, dokumentálni és amire kell használnunk.