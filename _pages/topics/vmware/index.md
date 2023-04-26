---
title: "Komponens diagram használata"
tags:
    - uml

---
## Rövid bemutatás

A UML (Unified Modeling Language) egy szoftverfejlesztésben előszeretettel használt modellező nyelv, mely diagramok, modellek létrehozására alkalmas.

A komponens diagram is egy UML diagram, azon belül szerkezeti diagram, ami egy adott szoftver szolgáltatásait és az azok közötti kapcsolatokat modellezi.

Egy **komponens** szolgáltatások egy halmazát foglalja magában.
Természetesen egy komponens tartalmazhat több kisebb komponenst is.
Ezek a komponensek egymást **interfészeken** keresztül érik el, és ezeken keresztül kommunikálnak egymással.
Az interfészeknek két fajtája van, **nyújtott interfész** és **megkövetelt interfész**. Míg az első a külvilág felé szolgáltatásokat nyújt, addig a második ezeket másoktól igényli.
A komponensek a külvilághoz **portokon** keresztül kapcsolódnak, és egy porthoz több interfész is kapcsolódhat.


![full_example](images/full_example.png)


## Jelölésrendszer

- **Komponens**: <\<component\>> előtaggal, vagy az alábbi jelöléssel
  ![component](images/component.png)


- **Interfész**: 
  - nyújtott interfész: körrel
  - megkövetelt interfész: félkörrel

  ![interface](images/interface.png)


- **Port**: téglalap, a komponens határvonalán
  ![port](images/port.png)


## Rajzprogramok

- [draw.io](http://draw.io/)
- [Lucidchart](https://www.lucidchart.com/pages/)
- [EdrawMax](https://www.edrawsoft.com/ad/edrawmax/)
- [PlantUML](https://plantuml.com/component-diagram)


## További hasznos linkek
- [Visual-Paradigma](https://www.visual-paradigm.com/guide/uml-unified-modeling-language/what-is-component-diagram/)
- [Wikipédia](https://en.wikipedia.org/wiki/Component_diagram)
- [YouTube Tutorial - UML 2 Component Diagrams](https://www.youtube.com/watch?v=KQUGFFN4M90&ab_channel=DerekBanas)
- [YouTube Tutorial az EdrawMax-hoz](https://www.youtube.com/watch?v=_iiOOxIDrGA&ab_channel=WondershareEdraw)