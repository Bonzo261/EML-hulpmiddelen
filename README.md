# EML-hulpmiddelen

Hulpmiddelen om testbestanden te maken en te controleren volgens de **EML_NL-standaard** van de Kiesraad: van verkiezingsdefinitie tot proces-verbaal. Elke app is één HTML-bestand dat helemaal in de browser draait. Er gaat niets naar een server.

> **Niet verbonden aan de Kiesraad.** Dit is een onafhankelijk hobbyproject. Het is niet gemaakt, gecontroleerd of goedgekeurd door de Kiesraad. Gebruik het alleen voor test en oefening, met fictieve gegevens. Controleer bestanden altijd tegen de officiële XSD's en modellen.

## Apps

| App | Bestand | EML |
|---|---|---|
| Verkiezingsdefinitie-maker | [`verkiezingsdefinitie.html`](verkiezingsdefinitie.html) | 110a |
| Stembureaus-maker | [`stembureaus.html`](stembureaus.html) | 110b (1.2.2 en 1.3.1) |
| Kandidatenlijsten-maker | [`kandidatenlijsten.html`](kandidatenlijsten.html) | 230b |
| Uitslagen-generator | [`uitslagen-generator.html`](uitslagen-generator.html) | 510b, 510c, 510d (optioneel ondertekend) |
| PV-generator | [`pv-generator.html`](pv-generator.html) | leest 110a/b, 230b, 510b/c/d/r, 520; maakt PDF |
| Controleprotocol | [`controleprotocol.html`](controleprotocol.html) | leest 110a/b, 230b, 510b/c/d |

De startpagina is [`index.html`](index.html), de gebruikershandleidingen staan in [`handleidingen.html`](handleidingen.html). Technische notities en bekende beperkingen per app staan in [`docs/`](docs/).

De volgorde waarin je de apps meestal gebruikt: **110a → 110b → 230b → 510 → PV / controleprotocol.**

## Gebruiken

- **Online:** via GitHub Pages (zie hieronder).
- **Lokaal:** download de repository en open `index.html` in de browser. Voor de PV-generator moet `pv-fonts.js` in dezelfde map staan.

Externe afhankelijkheden, geladen vanaf een CDN (daarvoor is internet nodig):
- Google Fonts (IBM Plex)
- [SheetJS](https://sheetjs.com/) (xlsx) voor Stembureaus-maker en Controleprotocol
- [pdfmake](http://pdfmake.org/) en [JSZip](https://stuk.github.io/jszip/) voor de PV-generator (van jsDelivr, met unpkg en cdnjs als reserve)

Sommige apps bewaren hun werk in `localStorage` van je browser. De Uitslagen-generator bewaart daar ook de testsleutels voor ondertekening. Die sleutels verlaten je browser alleen als je ze zelf exporteert. **Zet geëxporteerde sleutelbestanden (.json) nooit in deze repository.**

## GitHub Pages aanzetten

Settings → Pages → *Deploy from a branch* → `main` en map `/ (root)` → Save. Na een minuut staat de site op `https://bonzo261.github.io/EML-hulpmiddelen/`.

## Bronnen en dank

- De opbouw, teksten en opmaak van de processen-verbaal komen uit de Typst-sjablonen van [kiesraad/abacus](https://github.com/kiesraad/abacus) (`backend/templates`, EUPL-1.2).
- Het formaat van de 510-bestanden en de ondertekening volgen de testbestanden en de code van Abacus.
- PDF-lettertypen in `pv-fonts.js`: DM Sans (aangepast door de Kiesraad) en Geist Mono, beide onder de SIL Open Font License 1.1, uit `kiesraad/abacus`.

## Licentie

[EUPL-1.2](LICENSE), dezelfde licentie als Abacus.
