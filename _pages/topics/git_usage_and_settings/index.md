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

Ha már vannak commit-jaid, vagy másoknak vannak, akkor a `git pull` és `git push` parancsokkal lekérheted mások commit-jait, és feltöltheted a sajátaidat

Ha van olyan kódreészlet amit te is és más is módosított, akkor egy `merge conflict` fog keletkezni, és az érintett fileokban el kell döntened, hogy kinek a változtatásai maradjanak meg, magy egy hibrid változás keletkezzen

Ha rezolváltad a conflict-ot, akkor `git add <conflict-oló fileok>` majd `git commit` parancsokkal lezárhatod a merge-et

## Alap konfigurációs opciók
    - username, email, editor, remotes, `.git/config`, `.gitignore`

## Git müködése
### Commit-ok
### Branch-ek
### navigáció a history-ban
### bare repository-k
### Mit csinál a pull / push?

## Tippek
### git log
### git blame
### terminál kiegészítők
### git diff
### ssh-kulcsok
### kulcscsomó használata

