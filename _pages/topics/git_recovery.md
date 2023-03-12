# Git hibák helyreállítása
## Alapok
### Mégis mi egy commit valójában?
Az első dolog amit tudni kell a gitről, hogy *tartalomalapú*, nem pedig változtatásalapú, ellentétben például a Pijul vagy Darcs verziókezelőkkel.
Egy commit nem változtatásokat tartalmaz, hanem a repo mappa egy állapotát, plusz egy szöveges leírást, plusz a közvetlen előzmény commitot / commitokat.  Ebből generál a git egy azonosítót egy hash algoritmus segítségével, amire egy digitális ujjlenyomatként lehet gondolni.  Amit fontos tudni a mi szempontunkból a hashekről, hogy ha bármelyik változik a fenti bemenetekből, akkor a hash kimenete is változik, viszont ugyanarra a bemenetre mindig ugyanazt a kimenetet adja, tehát bár véletlenszerűnek néz ki a kimenet, valójában teljesen determinisztikus.

### Mi fán terem a branch?
A branch igazából csak egy név amit bármilyen commithoz rendelhetünk.  A tag kábé ugyanez, csak egy tag jó esetben létrehozása után örökké ugyanarra a commitra mutat, míg a branch lényege, hogy követi a történelem egy "fonalát" (vagyis inkább láncát), és újabb és újabb commitokra mutat.  Jó esetben ehhez a lánchoz csak újabb szemeket (commitokat) fűzünk.

## Véletlenül hozzáadott változtatás
### Szituáció:
Nem nézted át miket változtattál, futtattál mondjuk egy `git add .` parancsot, de még **nem** futtattad a `git commit`-ot.  Lefuttatod a `git diff --staged` parancsot és látod, hogy hoppá, nem az van benne aminek lennie kéne.

### Megoldás
A `git restore --staged file1 file2 ...etc` parancs visszaállítja a neki megadott fájlokat a `HEAD` commitból.  Ha nem egy teljes fájlt akarsz visszaállítani, akkor a `--patch` opcióval kiválaszhatod, hogy a `git diff --staged` kimenet mely részeit ("hunks") akarod visszaállítani.

## `HEAD` commit-ba valami rossz került bele / rossz a leírása
### Szituáció
Rosszul írtad le a commitodat, vagy esetleg rossz változtatások kerültek bele, vagy nem minden került bele.  Fontos: még **nem** futtattad a `git push` parancsot, **vagy** egy olyan branchen vagy amit mások nem használnak és senkit sem zavar a force pusholsz. (`git push --force`)

### Megoldás
A `git commit --amend` lecseréli a `HEAD` commitot, így ahelyett hogy lenne egy "add cool feature" commit és egyből utána egy "fix embarrassing typo in previous change" commit, csak egy "add cool feature" commit van typo nélkül.  A parancs amúgy ugyanúgy működik mint mindig, tehát a `git add` vagy `git commit --patch` hívása ugyanúgy szükséges a változtatások hozzáadásához, de ha csak a commit leírását akarod változtatni akkor nem.

## Megpróbáltam a rebase/merge/etc parancsot, de csak rosszabb lett minden, valami kigyulladt, a rendőrök már a házban vannak, mit tegyek, melyik országba meneküljek?
Mielőtt belekezdünk a rebase témába, érdemes tudni mit csinálj mikor garantáltan bajba jutsz az első próbálkozás során.

### --abort
Az első parancs a `git rebase --abort`.  Ez csak simán visszajuttat a rebase előtti állapotba.  Ez a pánik gomb.  Ha fogalmad sincs mi történt, ez kiment a szituációból és újrakezdheted a folyamatot egy ismert állapotból.  Hasonló módon van `git merge --abort`, `git cherry-pick --abort`, etc.

A második a `git reflog`, ait
## Rebase
Ez egy elég általános eszköz, ami veszélyes, de nagyon hasznos ha tudod mit csinálsz, és sok problémának a megoldása ebben rejlik.

### rebase vagy merge?
A `merge` egy speciális commitot készít, míg a `rebase` egy commit listát szerkeszt, bizonyos értelemben átírva a "történelmet".  Merge helyett is használható, nem csak hibajavításra, mert fast-forward merge-et tesz lehetővé.

### fast-forward???
TODO describe fast forward, probably move it to basics section
