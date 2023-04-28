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

### `--abort`
Az első parancs a `git rebase --abort`.  Ez csak simán visszajuttat a rebase előtti állapotba.  Ez a pánik gomb.  Ha fogalmad sincs mi történt, ez kiment a szituációból és újrakezdheted a folyamatot egy ismert állapotból.  Hasonló módon van `git merge --abort`, `git cherry-pick --abort`, etc.

## Rebase
Ez egy elég általános eszköz, ami veszélyes, de nagyon hasznos ha tudod mit csinálsz, és sok problémának a megoldása ebben rejlik.

### rebase vagy merge?
A `merge` egy speciális commitot készít, míg a `rebase` egy commit listát szerkeszt, bizonyos értelemben átírva a "történelmet".  Merge helyett is használható, nem csak hibajavításra, mert fast-forward merge-et tesz lehetővé.

### fast-forward?
Ha mergelés folyamán egy olyan commitot akarsz egy másikkal mergelni aminek felmenője az első commit, akkor a Git azzal egyszerűsíti le a dolgokat hogy a másodikra állítja a branch `HEAD`-et, mert nincsenek divergens változtatások amit mergelni kellene, ez a “fast-forward”.

Például, ha master nem divergált akkor egy új commit létrehozása helyett a master HEAD-jét az adott feature branch legutolsó commitjára állítja, így nem lesz merge commit sikeres fast forward esetén.

## Reset
A Git resetet alapvetően arra használjuk hogy egy adott branchet egy korábbi tetszőleges állapotra álítsunk vissza. Ennek három különböző verziója van: hard, mixed és soft.

Példának vegyük a következő commit historyt: `- A - B - C (master)`

### `--soft`
A parancs kiadása előtt a `HEAD` és az index is C-re mutat, B-re akarjuk soft módon visszaállítani.

A `git reset --soft B` parancs kiadása után a `HEAD` a B-re mutat, viszont az index tartalmazza a C commitban lévő változásokat.
A `git status` parancs a C-ben lévő változásokat staged-ként jelöli meg, ha kiadjuk a commit parancsot új commitot kapunk a C-ben lévő változásokkal

### `--mixed` (alapértelmezett)
A parancs kiadása előtt a `HEAD` és az index is C-re mutat, B-re akarjuk mixed módon visszaállítani.

A `git reset --mixed B` parancs hatására a `HEAD` B-re mutat és az index is B-re mutat. A C commitban lévő változások a lokális directory-ban unstaged állapotban jelennek meg, mivel az index B commit tartalmára mutat. Git add és git commit parancs kiadása után új commit jön létre a nekünk megfelelő változtatásokkal

### `--hard`
A parancs kiadása előtt a `HEAD` és az index is C-re mutat, B-re akarjuk hard módon visszaállítani.

`git reset --hard B` parancs `HEAD` és index változás szempontjából sokban hasonlít a mixed verzióra, azzal a lényeges különbséggel hogy hard mód esetén az összes C commitban lévő változás és a commitálatlan lokális változtatások is elvesznek. Ebből kifolyólag nagyon óvatosan kell használni a parancsot, kiadása előtt git statussal ellenőrizzük hogy van-e változás a lokális directory-ban illetve hogy megengedhető-e az adott commit utáni változások elvesztése.

