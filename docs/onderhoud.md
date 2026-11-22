# Onderhoud van de site

Statische website: alle apps zijn losse HTML-bestanden in de hoofdmap. Huisstijl: IBM Plex, Kiesraad-rood.

| Bestand | Inhoud |
|---|---|
| `index.html` | Startpagina met disclaimer, link naar de handleidingen en zes app-kaarten |
| `handleidingen.html` | Handleidingen per app (tabbladen via de hash, bijv. `#controleprotocol` of `#cp-s4`; prefixen vd, sb, kl, pv, ug, cp) |
| `verkiezingsdefinitie.html` | Verkiezingsdefinitie-maker (110a) |
| `kandidatenlijsten.html` | Kandidatenlijsten-maker (230b) |
| `stembureaus.html` | Stembureaus-maker (110b) |
| `uitslagen-generator.html` | Uitslagen-generator (510b/c/d) |
| `pv-generator.html` + `pv-fonts.js` | PV-generator |
| `controleprotocol.html` | Controleprotocol |

## Een app bijwerken
1. Vervang het HTML-bestand door de nieuwe versie.
2. Zet bovenaan in `<header class="top"><div>` de terugknop en de disclaimer terug:
   `<a class="back" href="index.html">← Alle hulpmiddelen</a><span class="nk"> · Onafhankelijk hulpmiddel, niet van de Kiesraad</span>`, met de CSS voor `.back` en `.nk` onderaan in `<style>`.
3. Komt de app uit een artifact met meer bestanden (zoals `pv-fonts.js` bij de PV-generator), neem die bestanden ook over.
4. In `handleidingen.html` verwijzen de knoppen "App openen" naar de lokale bestandsnamen.
5. Werk de handleiding en de notitie in `docs/` bij als de werking verandert.

## Afhankelijkheden buiten de site
- Google Fonts.
- cdnjs: xlsx (Stembureaus-maker en Controleprotocol).
- jsDelivr, met unpkg en cdnjs als reserve: pdfmake en jszip (PV-generator).
- `window.claude.use("downloads")` wordt alleen gebruikt als het beschikbaar is. Op een gewone website downloaden de apps via de browser.
