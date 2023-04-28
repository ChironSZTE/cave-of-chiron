---
title: "Git Használata és beállításai"
tags:
    - git
---

## Alapok

A `git` egy elosztott verziókövető rendszer

Minded git repository két részből áll:
  - `repository`: valójában csak a `.git` mappa maga a repository, de általában az egészet (annak a mappának az egészét, amiben van a `.git` mappa) annak szoktuk hívni
  - `working tree`: ez az összes `.git` mappa melletti file, amivel dolgozol

a git `elosztott`, mert mindenkinek aki dolgozik egy adott repo-ban, megvan a teljes repo-nak egy másolata: ez a `.git` mappa

amikor elkészítesz egy `commit`-ot, akkor a working tree egy "másolata" (nem egészen, de erről később van szó) elmentődik a te lokális repo-másolatodba

amikor `push`-olsz, akkor elküldöd egy távoli szerveren lévő repo-ba a te commitjaidat; ehhez hasonlóan amikor `pull`-olsz, akkor lekéred a mások által feltöltött commit-okat


## Alap használat
### Repository készítése

`git init` parancs készít egy lokális repot abban a mappában amiben vagy

`git clone <remote repo url>` lemásolja a `<remote repo url>` által mutatott repo-t abba a mappába amiben állsz

Az esetek többségében ez a `<remote repo url>` vagy egy `https://valami.com/username/repo` url, vagy egy `git@valami.com:username/repo` cím

Az másodikat az ssh kulcsos autchentikációval együtt kell használni, és így nem kell manuálisan felhasználónevet / jelszót megadni clone, push, pull és egyém műveleteknél

### Fejlesztés a repository-ban

Amikor készítesz egy változtatást, akkor azt a `git add <file(ok) neve>` parancsal a `staging area`-hoz hozzá tudod adni, majd ha elég változtatást csináltál, akkor az összes staging area-ban lévő változtatást elmentheted a repository-ba a `git commit` parancsal

Ha további argumentumok nélkül adot ki a `git commit`-ot, akkor a git meg fogja nyitni az alapértelmezett parancssoros szövegszerkesztődet (általában a `vim`-et, ez átállítható a `$EDITOR` környezeti változó beállításával) amiben szerkesztheted a `commit message`-et

Alternatívan a `git commit -m "<commit message>"`-el egysoros commit message-at bírunk megadni, anélkül hogy bármilyen szövegszerkesztőbe lépnénk


### Szinkronizálás távoli repository-val

Ha már vannak commit-jaid, vagy másoknak vannak, akkor a `git pull` és `git push` parancsokkal lekérheted mások commit-jait, és feltöltheted a sajátaidat.

Ha van olyan kódreészlet amit te is és más is módosított, akkor egy `merge conflict` fog keletkezni, és az érintett fileokban el kell döntened, hogy kinek a változtatásai maradjanak meg, magy egy hibrid változás keletkezzen.

Ha rezolváltad a conflict-ot, akkor `git add <conflict-oló fileok>` majd `git commit` parancsokkal lezárhatod a merge-et.

## Alap konfigurációs opciók
Két féle git konfigurációs lehetőség van: global és local.

A global az alapvetően minden git repo-ra érvényes azon a számítógépen.
A local config-okkal ezek a global opciók felülírhatók projeckt-specifikus értékekkel.
Az elején ezt be is kell állítanunk, hogy a későbbiek a lássa is a többi fejlesztő, hogy ki által történt a commit.
`git config --global user.name "neved"`
`git config --global user.email "email-ed"`
Érdemes viszont arra figyelni, hogy ilyenkor globálisan állítod be ezeket az adatokat. Ha egyszerre több külön projektre fejlesztesz más `repository`-kba akkor a global kifejezést elkell hagyni és az adott `git projekt` mappájában beállítani ezeket az adatokat.

Két féle config file-ban általában eltérő féle adatok vannak:
a global configban általában git plugin-ok, és felhasználó név / email értékek szoktak szerepelni,
a local configban ezek felülírhatók, és itt a jelen repo alapértelmezett vagy jelenleg kiválasztott remote-jai és branch-ei configurálhatók.

A global config általábna a `$HOME/.gitcongig` fileban található, míg a project-ek lokális configjai mindig az adott repo-ban lévő `.git` mappán belüli `config` fileban találhatók

A config file szintaktikailag viszonylag egyszerű:
```editorconfig
[szekció]
    név = érték
```
például egy global config:
```editorconfig
[credential]
    helper = /usr/lib/git-core/git-credential-libsecret
[core]
    excludesfile = /home/kissjoska/Projects/dotfiles/global_gitignore
[user]
    email = kissjoska@gmail.com
    name = kissjoska
```

például egy local config:
```editorconfig
[core]
    repositoryformatversion = 0
    filemode = true
    bare = false
    logallrefupdates = true
[remote "origin"]
    url = git@github.com:ChironSZTE/cave-of-chiron.git
    fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
    remote = origin
    merge = refs/heads/master
[branch "20-git-usage-and-settings"]
    remote = origin
    merge = refs/heads/20-git-usage-and-settings
```

A git által használt szövegszerkeztő (amit alapból commit-oláskor nyit meg) az a `$EDITOR` környezeti változóval befolyásolható, `vim` az alapértelmezett (ha ez a környezeti változó nem lenne beállítva)

A `.gitignore` file-al befolyásolható, hogy mely file-mintákat hadja figyelmen kívül a git, amikor az `untracked` fileokat vizsgálja (már tracked fileokat nem érint a gitignore).
A file nagyon egyszerű, mindössze soronként egy mintát tartalmaz.
A mintákról részletesen a [git dokumentációban (https://git-scm.com/docs/gitignore)](https://git-scm.com/docs/gitignore) olvashatsz

## Git müködése
### Commit-ok

A háttérben valójában nem az egész `working tree` másolata mentődik el egy commit-ba, hanem csak a `diff`.

A `diff` az egy linux program, ami szöveges fileok összehasonlítását végzi; ebből kifolyólag, egy commitban, csak az őt megelőző commit-hoz képest történt változások kerülnek mentésre.

A `git diff <egyik commit hash>..<másik commit hash>` parancsal a két commit használat között történt nettó változást nézhetjük meg, a két commit hash nem kell hogy "szomszédok" legyenek, vagy hogy egyáltalán ugyan azon a branch-en legyenek.

Ezt a parancsot egy fileba is kiirányíthatjuk, és azt szerkezthetjük, és vissza apply-olhatjuk is akár.

A `commit message` minél érthetőbb és lényegretörőbb legyen. 
Ez azért fontos, mert a későbbiekben gyorsan meglehessen mondani, hogy annál a `commit`-nál mit fejlesztettünk, változtattunk. 
A többi fejlesztő számára is hasznos és akik ezeket nézik / ellenőrzik számukra is egyszerűbb ha nem kell átnéznie a teljes `commit`-ot hanem a message-et olvasva értik, hogy ott mi történt.

Érdemes ehhez haszálnunk különböző kulcsszavakat a `message` elejénél:
- `fix`: Hiba javítása
- `feat`: Új funkció
- `docs`: Csak dokumentációs változtatások
- `style`: Olyan változások, amelyek nem érintik a kód jelentését (szóközök, formázás, hiányzó pontosvesszők stb.)
- `refactor`: Kódváltoztatás, amely nem javít hibát és nem ad hozzá új funkciót
- `test`: Hiányzó tesztek hozzáadása
- `chore`: Változások a build folyamatban vagy kiegészítő eszközökben és könyvtárakban, például dokumentáció generálásban.

Példa: `feat: Implement user authentication`

**git commit használata**
Commit írásánál az alábbi parancsot futassuk le:
`git commit -m "message"`

### Branch-ek

A branch-ek lehetővé teszik, hogy egyszerre több ember dolgozzon párhuzamosan, és ezek ne akadjanak össze, vagy ha mégis, akkor egy lépésben (merge) le lehet kezelni az esetleges conflict-okat, és nem kell minden commitnál ezzel bajlódni.

Egy új branch-et a `git branch <branch neve>` parancsal hozhatunk létre, ami alapból ott fog állni, ahol a parancs kiadásakor álltunk.

Ahhoz hogy ezen a branch-on bírjunk dolgozni, át kell rá váltani a `git switch <branch neve>` vagy `git checkout <branch neve>` parancsok egyikével.

Általában az alábbi `branching modell`-t szokták használni:
- `feature branches`: új funkcióknak szóló branchek
- `develop`: a fő fejlesztési branch
- `release branches`: itt történnek a finomítások és a metódusok ellenőrzése
- `hotfixes`: hibák javítása
- `master`: a kész működő rész. Ide csak a jól működő részeket `merge`-eljük be

### navigáció a history-ban

A history-ban való navigázióra számos mód van:

A `git log` parancsal visszanézhetünk a commitokra
A `git log --oneline --graph` paraméterekkel egy tömörebb nézetet is kaphatunk

A `git switch <branch neve>` parancsal átválthatunk egy másik branch-re

A `git checkout <branch neve, commit hash vagy címke>` parancsal átváltahatunk egy másik branch-re, vagy egy adott commit-ra (ez úgynevezett detached HEAD módba rakja a repo-t, ahol csak körülnézni bírunk), vagy egy adott címkére (a HEAD az például egy címke, de lehet sajátokat létrehozni, ezt általában verziószámozásra szokták használni)


### bare repository-k

A `bare` repository-k olyan repo-k, amelyeknek nincsen working tree-jük, hanem csak maga a .git mappa tartalma (maga a repository) van egy könyvtárban.
Git szervereken (pl GitHub, GitLab) ilyen repo-k vannak, ugyanis ha egy repo-hoz tartozik working tree, akkor a git nem enged abba a repo-ba push-olni

### Mit csinál a pull / push?

A git push a mi commit-jainkat elküldi egy távoli git szerverre

A git pull az lekéri az összes változást (`git fetch --all`) majd megpróbál átváltani a legújabb commit-ra.

Ezt többféleképpne teheti meg, és az alapértelmezett viselkedés konfigurálható:

  - `fast-forward`: amikor neked nincs változtatásod (commit-od) akkor egyszerűen beállítja a jelenlegi branch-ot és a HEAD-et a legutóbbi commit-ra
  - `rebase`: amikor a te commitjaidat egyesével "rápakolja" a letöltött változások utánra, mintha már az új változások ott lettek volna amikor elkészítetted a változásaidat
  - `merge`: amikor a git megpróbálja összeolvasztani a változásokat (egy ilyen `merge commit` segítségével, aminek az összes többi commit-al ellentétben 2 szülő commit-ja van); amikor az automatikus összeolvasznás nem sierül, akkor egy `merge conflict`-ot kapunk, és a git az ütköző régiókba beleteszi mindkettő variációt, és nekünk kell feloldani a conflict-ot az egyik, másik vagy egy hibrid megoldás elkészítésével

#### Feltöltés folyamata
- Egy adott rész befejezésénél a változtatásokat `git add <file név>` vagy ha az összes változtatást szeretnénk feltölteni `git add .`
- A következő lépés a `commit message` megírása `git commit -m "message"` paranccsal.
- A `git status` paranccsal megtudjuk nézni mely változtatások kerültek `staging area`-ba.
- A `git push` parancs kiadásával a `stage`-elt fájljainkat töltjük fel a local repository-ból a távoli repository-ba. 

A `stage` egy köztes állapot ahol úgymond megjelölt módosított fájl/fájlok vannak. Ezeket a `git add`-al tudjuk megjelelölni. Ezeket töltjük majd fel a `git push` parancs kiadásával.

## Tippek
### git blame

A `git blame <file név>` parancs-al menézhetjük, hogy abban a fileban egy adott sort ki módosított utoljára

### Terminál kiegészítők

Ha a terminálban vagyunk, sokszor nem egyértelmű, hogy az adott mappa amiben állunk, az egy git repo-e vagy nem, és ha igen akkor nem tudunk róla semmit

Erre van számos shell-hez (`bash`, `zsh`, `fish`) vannak kiegészítők, hogy hasznos információkat automatikusan megjelenítsenek a shell promt-ban

A git-el alapból kapunk egy alap promt kiegészítőt bash-hez, ezt csak be kell kapcsolni; több infó a [git dokumentációban (https://git-scm.com/book/en/v2/Appendix-A%3A-Git-in-Other-Environments-Git-in-Bash)](https://git-scm.com/book/en/v2/Appendix-A%3A-Git-in-Other-Environments-Git-in-Bash)

Ezek általában a git repo állapotát tartalmazzák: jelenlegi branch, módosított fileok száma, nem feltöltött commitok száma, és hasonlók

### Automatikus autentikáció

Alapból ha valahonnan `clone`-oltuk a repo-t, akkor ha oda fel akarjuk tölteni a változtatásainkat, akkor valamien módon autentikálni kell magunkat, hogy biztosan van-e jogunk oda feltölteni

Ez az esetek nagyrészében email-jelszó párossal tehetjük meg, de ha ezt használjuk, akkor minden push és pull előtt autentikálni kell megunkat.

Ez megkerülhető ssh-kulcsok használatával, vagy a jelszavak megjegyeztethetők a rendszer kulcscsomójára

#### ssh-kulcsok

Ehhez generálnunk kell magunknak egy public/private kulcspárt, és a publikus kulcsot fel kell vinni a távoli szerverre, és a megfelelő repo-ban a remote-ot át kell állítani az ssh-t használó url-re (amik általában a `git@github.com:ChironSZTE/cave-of-chiron.git` féle mintát követik)

Részletes dokumentációk elérhetőek [github-hoz (https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) és [gitlab-hoz (https://docs.gitlab.com/ee/user/ssh.html)](https://docs.gitlab.com/ee/user/ssh.html) is

#### kulcscsomó használata

Ha az adott távoli git szerver ezt nem támogatná, vagy a 22-es ssh port zárva van, vagy nincs kedvünk kulcsokat generálni, akkor az eddig használt felhasználónév-jelszó párost elmenthetjük a rendszer kulcscsomójára

számos linux distro-n elérhető több `libsecret`-et használó megoldás, a két legnépszerűbb a `gnome-keyring` (a hozzá tartozó GUI a `seahorse` néven elérhető) és a `kde-wallet` (bármely kde desktop-al alapból telepítve van, GUI-val együtt)

Ahhoz hogy ezt használhassuk, csupán a git plugint kell bekapcsolni: 
```shell
git config --global credential.helper /usr/lib/git-core/git-credential-libsecret
```
És amikor legközelebb autentikáljuk magunkat, akkor a git automatikusan elmenti a bejelentkezési adatokat, és a továbbiakban automatikusan felhasználja őket a kulcscsomóról;
több részlet a [Gnome keyring arch wiki (https://wiki.archlinux.org/title/GNOME/Keyring#Git_integration)](https://wiki.archlinux.org/title/GNOME/Keyring#Git_integration) oldalán olvasható

#### git tanulását, megértését segítő oldalak

Egy nagyon hasznos oldal ahol játékosan tanulhatjuk a git parancsokat és segít a megértésében is ezeknek lépésről lépésre.
[Learn Git Branching](https://learngitbranching.js.org/)

Step by step guide a GitHub-hoz és hogy hogyan használjuk a git-et
[Step by step guide](https://opensource.com/article/18/1/step-step-guide-git)

A legismertebb git branching modell
[Git branching](https://nvie.com/posts/a-successful-git-branching-model/)

#### git kezelését segítő programok

A GitHub-nak van egy saját desktop alkalmazása:
[GitHub Desktop](https://desktop.github.com/)

A Git Extensions egy UI felület a Git Repository-khoz ahol látjuk a commit-okat és a workflow-t
[Git Extensions](http://gitextensions.github.io/)

A Sourcetree egy egyszerű Git GUI alkalmazás
[Sourcetree](https://www.sourcetreeapp.com/)


