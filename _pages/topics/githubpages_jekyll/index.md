---
title: "GitHub Pages és Jekyll"
tags:
    - jekyll
    - web development
    - static site
    - github
    - github pages
---

## Alapok

A [Jekyll](https://jekyllrb.com/) egy statikus weboldal generáló motor. [GitHub Pages](https://pages.github.com/) egy a [GitHub](https://github.com/)-hoz kapcsolódó szolgáltatás, mely a GitHub projektekhez (Git repozitoriban) tárolt statikus weboldalakat szolgálja ki. A GitHub Pages közvetlenül támogatja a Jekyll-el készült oldalakat.

### Témák

A Jekyll oldalak alapvető szerkezetét és bizonyos (főleg grafikus felülethez kapcsolódó) funkciókat a témák határozzák meg. Egy összetett témára példa az ezen oldalon is használt [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/).

## Telepítés

### Windows 10

Windows alatt a [Ruby](https://www.ruby-lang.org/en/) 2 főverziójú telepítésevel használható, bár a honlap ennél [tágabb verzió tartományt ad meg](https://jekyllrb.com/docs/#prerequisites).
Ruby-val támogatott parancssor a Start menűből indítható.

![start-menu-ruby-console](screenshots/start-menu-ruby-console.png)

## Futtatás

### Saját gépen (Windows 10)

1. Ruby parancssor indítása.
2. Szerezzük be a projekt forráskódját (például, [Git]({{ site.baseurl }}/tags#git) verziókövetőn keresztül).
3. Navigáljunk el a Jekyll oldalunk gyökerébe, ahol a [_config.yml]({{ site.data.global_links.project.github.root }}/blob/master/_config.yml) található.
4. Szükség estén telepítsük a hiányzó függőségeket [bundler](https://jekyllrb.com/docs/ruby-101/) segítségével.
5. Indítsuk el a Jekyll fejlesztői szervert a `bundle exec jekyll serve` vagy `jekyll serve` parancssal (némelyik rendszeren csak az utóbbi működik).
6. Frissítsük az oldalt a böngészőnkben. Javasolt a `ctrl + f5` használata, hogy felülbíráljuk a böngésző gyorsítótárát és a legfrissebb változatot kérjük le a szervertől.

A fejlesztői szerver alapértelmezésként nem figyeli a config fájl változásait. Ezért ennek módosítása esetén újra kell indítani.
{: .notice--warning}

### Github Pages

A [GitHub projekt beállításainál aktiválás](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site) után egyéb teendőnk nincsen a várakozáson kívül. A GitHub Pages legenerálja és a megfelelő címen elérhetővé teszi az oldalunkat. A folyamat akár percekbe is telhet. Frissítsük az oldalt a böngészőnkben. Javasolt a `ctrl + f5` használata, hogy felülbíráljuk a böngésző gyorsítótárát és a legfrissebb változatot kérjük le a szervertől.