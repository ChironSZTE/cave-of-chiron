---
title: "A markdown github változatának használata"
tags:
- Markdown
---

## Címsorok
A markdownban a # karaktert használva megadhatunk különböző szintő címsorokat,
a felirat annál kisebb lesz minél több # karaktert használunk. A legalacsonyabb szintű a 6 -os.
Használatukkor figyelnünk kell arra hogy a # után kerüljön `space`.
#### Példa:
# 1.szintű
## 2. szintű
### 3. szintű
#### 4. szintű

## Szöveg taglalás
A szövegben új sorba írhatunk ha a 'space' billentyűt kétszer lenyomjuk az utolsó sorban.
Ez úgy is elérhető ha a sorok végén a `<br />` taget használjuk. 
A markdown képes a legtöbb HTML tag használatára,   
pl a kommentek is a HTML- formátumból jönnek
`<!-- -->`
Új bekezdést egy sor kihagyásával kezdhetünk.

## Szöveg kiemelése
A szövegben a `*` karakterek használatával különböző formázásokhoz juthatunk.  
*Italic* `*Italic*`  
**Bold** `**Bold**`  
***Bold and italic*** `***Bold and italic***`  
~~dashed~~ `~~dashed~~`  
abban az esetben ha egy speciális karakter akarunk kiiratni, használhatjuk a \ -t .

## Listák és táblázatok
### Rendezett lista
A rendezett listákhoz az elemeket el kell választanunk `Enter` -el és a szám. elem
formátumot kell kövesse pl:
1. elem
2. elem

fontos megjegyezni hogy a markdown nem a leírt számot jeleníti meg hanem mindig a sorrendben következőt azaz
ha lenne egy listám aminek minden elemére az 1. elemet írnám akkor 
az eredményben ez 1. elem 2. elem stb -ként jelenne meg. 
Ahhoz hogy az írást nem listaszerűen folytatni tudjuk egy táblázat után kell csinálnunk egy úly bekezdést.
### Rendezelen lista
A rendezetlen listákat a `-` után adhatunk meg. 
#### Példa
- elem
- elem

## Speciális blokkok
### Idézet
A szövegbe ha idézeteket akarunk beszúrni akkor a `>` karakter után tudjuk ezt megadni
#### Példa
>Ez egy idézet aminek a taglalását a szöveg autómatikusan lekezeli. Bármilyen hosszú is legyen, a sortörést akkor is megoldja.

Az idézetek esetén minden sornak ezzel a karakterrel kell kezdődnie.

### Kód
Megadhatunk kód blokkokat is a \```  \``` karakterek  között.
```java
//komment
private int alma=1;
```
itt ha közvetlen az első \`\`\` után odaírjuk a nyelvet is akkor ez alapján fogja a szintaxist kiemelni.
Ezt alkalmazhatjuk soron belül is egy darab \`\` után is tehát így: \`\`tagname\`\` ez egy példában így néz ki: ``tagname``

## Hivatkozások
### Képek
A szövegben hivatkozhatunk képeket ezt gitlabon úgy tehetjük meg hogy az `attach a file or image` opciót választjuk
a komment-doboz jobb felső sarkában. Ez egy hivatkozást fog beilleszteni, de előtte egy `![képnév]` -t.
### Linkek
Ha linket akarunk beszúrni és el is nevezni akkor a következő szintaxist kell használni:
```
![elnevezés a szövegben](link)
```




