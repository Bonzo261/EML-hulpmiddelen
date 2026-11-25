# Onderhoud van de site

Statische website: alle apps zijn losse HTML-bestanden in de hoofdmap. Huisstijl: IBM Plex, Kiesraad-rood.

| Bestand | Inhoud |
|---|---|
| `index.html` | Startpagina met logo, uitleg, disclaimer, link naar de handleidingen en zeven app-kaarten in drie groepen (basisbestanden maken, uitslagen testen, uitslag controleren) |
| `handleidingen.html` | Handleidingen per app (tabbladen via de hash, bijv. `#controleprotocol`, `#cp-excel` of `#zetelverdeling-controle`; prefixen vd, sb, kl, pv, ug, cp, zv) |
| `verkiezingsdefinitie.html` | Verkiezingsdefinitie-maker (110a) |
| `kandidatenlijsten.html` | Kandidatenlijsten-maker (230b, 230c, 210) |
| `stembureaus.html` | Stembureaus-maker (110b) |
| `uitslagen-generator.html` | Uitslagen-generator (510b/c/d) |
| `pv-generator.html` + `pv-fonts.js` | PV-generator |
| `controleprotocol.html` | Controleprotocol (GR26) |
| `zetelverdeling.html` | Controle zetelverdeling (CSB, P 5–P 10) |
| `logo-512.png`, `favicon.ico`, `favicon-16.png`, `favicon-32.png`, `apple-touch-icon.png` | Logo en iconen |

## Startpagina en logo
De startpagina (`index.html`) en de logobestanden worden bijgehouden in een apart artifact. Bij het overnemen moet de `<title>` terug naar `EML-hulpmiddelen voor verkiezingen – hobbyproject`.

## Een app bijwerken
1. Vervang het HTML-bestand door de nieuwe versie.
2. Zet bovenaan in `<header class="top"><div>` de terugknop en de disclaimer terug:
   `<a class="back" href="index.html">← Alle hulpmiddelen</a><span class="nk"> · Onafhankelijk hulpmiddel, niet van de Kiesraad</span>`, met de CSS voor `.back` en `.nk` onderaan in `<style>`.
2c. In `handleidingen.html` staat een tab over de benoemingen-app. Die app heeft een eigen repository en hoort niet op deze site: haal de tab, de kaart, de regel in het schema en de links ernaar weg (zie `views`/`prefix` in het script).
2b. Zet vóór `<title>` de vier icon-links (favicon.ico, favicon-32.png, favicon-16.png, apple-touch-icon.png); zie `index.html`.
3. Komt de app uit een artifact met meer bestanden (zoals `pv-fonts.js` bij de PV-generator), neem die bestanden ook over.
4. In `handleidingen.html` verwijzen de knoppen "App openen" naar de lokale bestandsnamen.
5. Werk de handleiding en de notitie in `docs/` bij als de werking verandert.
6. Beschrijf de wijziging in `CHANGELOG.md`.
7. Haal afgeronde punten weg uit `WENSEN.md` en zet nieuwe wensen erbij.

## Afhankelijkheden buiten de site
- Google Fonts.
- cdnjs: xlsx (Stembureaus-maker). Het Controleprotocol schrijft en leest Excel zelf, zonder externe bibliotheek.
- jsDelivr, met unpkg en cdnjs als reserve: pdfmake en jszip (PV-generator).
- `window.claude.use("downloads")` wordt alleen gebruikt als het beschikbaar is. Op een gewone website downloaden de apps via de browser.
