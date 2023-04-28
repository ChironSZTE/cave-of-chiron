# Git hibák helyreállítása
## Alapok
Itt pár alapfogalmat ismétlünk, mivel a bevezetők és tutorialok sokszor csak egy felületes magyarázatot adnak, ami hibák javításakor téves mentális modellhez tud vezetni.

### Mégis mi egy commit valójában?
Az első dolog amit tudni kell a gitről, hogy *tartalomalapú*, nem pedig változtatásalapú, ellentétben például a Pijul vagy Darcs verziókezelőkkel.
Egy commit nem változtatásokat tartalmaz, hanem a repo mappa egy állapotát, plusz egy szöveges leírást, plusz a közvetlen előzmény commit / commitok azonosítóit.  Ebből generál a git egy azonosítót egy hash algoritmus segítségével, amire egy digitális ujjlenyomatként lehet gondolni.  Amit fontos tudni a mi szempontunkból a hashekről az az, hogy ha bármelyik változik a fenti bemenetekből, akkor a hash kimenete is változik, viszont ugyanarra a bemenetre mindig ugyanazt a kimenetet adja, tehát bár véletlenszerűnek néz ki a kimenet, valójában teljesen determinisztikus.
Konyhanyelvre fordítva: egy commit nevét a commit tartalma és előzménye határozza meg.

### Mi fán terem a branch?
A branch igazából csak egy név amit bármilyen commithoz rendelhetünk.  A tag kábé ugyanez, csak egy tag jó esetben létrehozása után örökké ugyanarra a commitra mutat, míg a branch lényege, hogy követi a történelem egy "fonalát" (vagyis inkább láncát), és újabb és újabb commitokra mutat.  Jó esetben ehhez a lánchoz csak újabb szemeket (commitokat) fűzünk.

### `merge`
A jelenlegi és egy adott másik branch szétválása óta történt változásokat fésüli össze arra a branchre amin jelenleg vagyunk. Többekközött a git pull is ezt használja hogy a remote repositoryba feltöltött változásokat beleolvassza a local repository kódjába, de bármelyik két tetszőleges branch változásait összefésülhetjük. Különbség a rebase-héz képest hogy a merge nem írja át/fésüli össze a két branch commit historyát az összegzés során, csak létrehoz egy merge commitot amiben az összes változás egyszerre jelenik meg.

## Diagnózis
### `status`
A `git status` a legfontosabb parancs, nem azt mondja meg, hogy milyen fájlok változtak és mely változtatások lettek hozzáadva a staging arénához, hanem azt is, hogy milyen művelet van folyamatban, és hogy a git tőleg mit vár el mielőtt továbbléphetnél.

Például ha konfliktus alakult ki valamelyik fájlban, akkor azt itt láthatod.

### `show` és `log`
Érdemes sűrűn visszanézni, hogy a történelem tényleg azt tartalmazza-e amit kellene, ebben segítenek ezek a parancsok.

Ajánlott mindkettőnek a formátum opcióit megismerni, az egyik leghasznosabb saját tapasztalatom szerint a `git log --patch`, amivel át tudod nézni, hogy a változtatások megfelelnek-e a commitok leírásainak.

#### távoli branchek állapota
Merge, rebase, vagy cherry-pick esetén néha a más fejlesztők branchén talált commit leírása nem elég ahhoz, hogy a sajátunkkal integráljuk.  Ilyenkor a "mire gondolt a költő" misztériumát sokszor feloldja, ha megnézzük mi a commit (vagy a változtatott fájl vagy mappa) történelme.

Ezeket egy `git fetch` futtatásával tudjuk lokálisan elérhetővé tenni.  Az  `origin` remote `main` branchét például `git fetch origin && git log origin/main` módon tudjuk átnézni.

### `reflog`
A reflog a jelenlegi branch lokális történelme.  Míg a `git log` a jelenlegi `HEAD` commit "családfáját" mutatja be, a reflog a branchen végzett (a `HEAD` célpontját változtató) műveletek időrendi sorrendjét követi.  Ez azért fontos, mert olyan műveleteket is követ amik átírják vagy akár törlik a branch "történelmét".

Ha például befejezted a `rebase`-t, vagy rosszul használtad `reset --hard`-ot, akkor meg tudod nézni mi volt az előző commit ahova a `HEAD` mutatott, és átirányíthatod a branchet hogy arra mutasson.

A formátumát itt részletesen nem tárgyaljuk, a kimenete eléggé önleíró.  A sor eleje mindig az "eredmény" commit ahova a sorban leírt művelet után került a branch, maga a szöveges leírás pedig általában elég ahhoz, hogy tudd mi volt a konkrét művelet.

## Véletlenül hozzáadott változtatás
### Szituáció
Nem nézted át miket változtattál, futtattál mondjuk egy `git add .` parancsot, de még **nem** futtattad a `git commit`-ot.  Lefuttatod a `git diff --staged` parancsot és látod, hogy hoppá, nem az van benne aminek lennie kéne.

### Megoldás
A `git restore --staged file1 file2 ...etc` parancs visszaállítja a neki megadott fájlokat a `HEAD` commitból.  Ha nem egy teljes fájlt akarsz visszaállítani, akkor a `--patch` opcióval kiválaszhatod, hogy a `git diff --staged` kimenet mely részeit ("hunks") akarod visszaállítani.

## `HEAD` commit-ba valami rossz került bele / rossz a leírása
### Szituáció
Rosszul írtad le a commitodat, vagy esetleg rossz változtatások kerültek bele, vagy nem minden került bele.  Fontos: még **nem** futtattad a `git push` parancsot, **vagy** egy olyan branchen vagy amit mások nem használnak és senkit sem zavar ha force pusholsz. (`git push --force`)

### Megoldás
A `git commit --amend` lecseréli a `HEAD` commitot, így ahelyett hogy lenne egy "add cool feature" commit és egyből utána egy "fix embarrassing typo in previous change" commit, csak egy "add cool feature" commit van typo nélkül.  A parancs amúgy ugyanúgy működik mint mindig, tehát a `git add [--patch]` vagy `git commit --patch` hívása ugyanúgy szükséges a változtatások hozzáadásához, de ha csak a commit leírását akarod változtatni akkor nem.

## Megpróbáltam a rebase/merge/etc parancsot, de valami katasztrofális történt, mit csináljak?
Mielőtt belekezdünk a rebase témába, érdemes tudni mit csinálj mikor garantáltan bajba jutsz az első próbálkozás során.

Az első parancs a `git rebase --abort`.  Ez csak simán visszajuttat a rebase előtti állapotba.  Ez a pánik gomb.  Ha fogalmad sincs mi történt, ez kiment a szituációból és újrakezdheted a folyamatot egy ismert állapotból.  Hasonló módon van `git merge --abort`, `git cherry-pick --abort`, etc.

A másik a reflog, amit fentebb már tárgyaltunk, de fontos még egyszer kiemelni, mivel nagyon hasznos.

## Rebase
Ez egy elég általános eszköz, ami veszélyes, de nagyon hasznos ha tudod mit csinálsz, és sok problémának a megoldása ebben rejlik.

### rebase vagy merge?
A `merge` egy speciális commitot készít, míg a `rebase` egy commit listát szerkeszt, bizonyos értelemben átírva a "történelmet".  Merge helyett is használható, nem csak hibajavításra, mert fast-forward merge-et tesz lehetővé.

### fast-forward?
Ha mergelés folyamán egy olyan commitot akarsz egy másikkal mergelni aminek felmenője az első commit, akkor a Git azzal egyszerűsíti le a dolgokat hogy a másodikra állítja a branch `HEAD`-et, mert nincsenek divergens változtatások amit mergelni kellene, ez a “fast-forward”.

Például, ha master nem divergált akkor egy új commit létrehozása helyett a master HEAD-jét az adott feature branch legutolsó commitjára állítja, így nem lesz merge commit sikeres fast forward esetén.

### interaktív mód
Alapértelmezetten a rebase nem interaktív, vagyis nem ad beleszólást abba, hogy milyen sorrendben alkalmazza a commitokat, se azok szerkesztésére, de ha az `--interactive` opcióval indítjuk el, akkor egy úgynevezett "to do" listát kapunk, ahol nagyon sok mindenbe van beleszólásunk.

### diagnózis
A jelenlegi állapotot és a legutúbbi pár elvégzett műveletet `git status` is megmondja, de ha látni szeretnéd az összes elvégzett műveletet, akkor a `.git/rebase-merge/done` fájlt nyisd meg.  Ez csak rebase közben elérhető, utána a mappa törlődik.

### commitok átrendezése
Az egyik legegyszerűbb művelet a változtatások átrendezése más sorrendbe.  Ezt például azért akarhatjuk csinálni, mert az egyik commit nem tesztelhető egy utána következő nélkül, és könnyebbé szeretnénk tenni a review folyamatot azzal, hogy minden commit egyenként futtatható legyen, ami cherry pickelést is egyszerűsíti, ha arra jutna a sor.

### squash, autosquash
A squash több egymás után következő commitból egyet csinál, az autosqash specifikusan arra való, hogy fixup commitokat (lásd a `commit` parancs `--fixup` opcióját) squasholj.  Az utóbbi nem kérdezi, hogy akarod-e változtatni a squasholt commit leírását, az előbbi igen.

### commitok eldobása
Ha nem akarsz egy commitot használni mégse, mert kiderült, hogy felesleges, és például a merge request már nem egy változtatásra fókuszálna vele együtt, akkor ki is hagyhatod a rebase során, egyszerűen töröld a sorát a listából, vagy kommentáld ki.

### the Dark Souls of commitok átrendezése
Van egy advanced módja a commitok átrendezésének, ez az, amikor már egy rebase közepén vagy, és rájössz, hogy egy már elvégzett művelethez tartozó commitnak később kéne következnie, mert például nem tesztelhető önmagában, és szeretnéd ezt lehetővé tenni, mert akár kötelezi ezt a projekt amin dolgozol.

Ez elég rizikós, de megoldható, csak végig tartsd fejben, hogy tényleg elvégezted a műveletet, vagy mondjuk konfliktus keletkezett és a Git nem engedett tovább.

A munka menete nagyjából úgy néz ki, hogy észreveszed hogy a jelenlegi commit nem buildel, megnézed melyik későbbi commit tartalmazza a változtatást amitől működne (például egy hiányzó deklarációt), megkeresed a `done` fájlban, kivágod onnan, futtatod a `git rebase --edit-todo` parancsot, hozzáadod a megfelelő commit után amit kivágtál az előbb, futtatsz egy `git reset --hard <commit>` parancsot, ahol a commit a legutóbbi befejezett rebase parancs **eredménye**.

Fontos, hogy ezt a logból tudod megkapni, **nem** a `done` fájlból.  Nem teljesen intuitív, de ha visszaemlékszel a bevezetőből a commitokra, akkor talán már érted is, hogy miért kell erre figyelni.  A rebase általában új történelmet kreál, amiben a commitoknak még ha a `--patch` nézete meg is egyezik, az őseik például mások, ezért nekik is más ellenőrzőösszegük lesz.  Tehát ha a `done` fájlból kapott hashre resetelsz, akkor az egész eddigi rebase-t gyakorlatilag a kukába dobod.

Ha a resetet jól csináltad, akkor futtathatod a `git rebase --continue`-t és haladhatsz tovább.  A biztonság kedvéért itt mindig ellenőrizd, hogy a `git log --patch` kimenete tényleg az-e amire számítottál.  Nagyon kellemetlen egy nagy rebase-t az elejéről újrakezdeni.

## Reset
A Git resetet alapvetően arra használjuk hogy egy adott branchet egy tetszőleges állapotra álítsunk vissza.

### mikor használható?
A legegyszerűbb használati eset az, amikor a branch előzményéből próbálunk visszaállítani egy állapotot, de használhatjuk például akkor is, ha el akarjuk dobni a lokális változtatásainkat és a branchet visszaállítani a remote állapotára, például mert egy rendes merge vagy rebase nem érné meg a fáradságot.  A `--hard`-on kívüli módok hasznosak akkor, ha például rebase közben egy commitot fel szeretnénk szeletelni kisebb egységekre, lásd feljebb.

### reset módok
Három különböző verziója van: hard, mixed és soft.

Példának vegyük a következő commit historyt: `- A - B - C (main)`

#### `--soft`

A parancs kiadása előtt a `HEAD` és az index is C-re mutat, B-re akarjuk soft módon visszaállítani.

A `git reset --soft B` parancs kiadása után a `HEAD` a B-re mutat, viszont az index tartalmazza a C commitban lévő változásokat.
A `git status` parancs a C-ben lévő változásokat staged-ként jelöli meg, ha kiadjuk a commit parancsot új commitot kapunk a C-ben lévő változásokkal

#### `--mixed` (alapértelmezett)
A parancs kiadása előtt a `HEAD` és az index is C-re mutat, B-re akarjuk mixed módon visszaállítani.

A `git reset --mixed B` parancs hatására a `HEAD` B-re mutat és az index is B-re mutat.  A C commitban lévő változások a lokális directory-ban unstaged állapotban jelennek meg, mivel az index B commit tartalmára mutat.  Git add és git commit parancs kiadása után új commit jön létre a nekünk megfelelő változtatásokkal

#### `--hard`
A parancs kiadása előtt a `HEAD` és az index is C-re mutat, B-re akarjuk hard módon visszaállítani.

`git reset --hard B` parancs `HEAD` és index változás szempontjából sokban hasonlít a mixed verzióra, azzal a lényeges különbséggel hogy hard mód esetén az összes C commitban lévő változás és a commitálatlan lokális változtatások is elvesznek.  Ebből kifolyólag nagyon óvatosan kell használni a parancsot, kiadása előtt git statussal ellenőrizzük hogy van-e változás a lokális directory-ban illetve hogy megengedhető-e az adott commit utáni változások elvesztése.

