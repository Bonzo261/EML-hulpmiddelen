# EML-tools

*Voorheen EML-hulpmiddelen. De repository en het webadres hebben de oude naam nog.*

Hulpmiddelen om testbestanden te maken en te controleren volgens de **EML_NL-standaard** van de Kiesraad: van verkiezingsdefinitie tot proces-verbaal. **Direct gebruiken: <https://bonzo261.github.io/EML-hulpmiddelen/>**

Elke app is één HTML-bestand dat helemaal in de browser draait. Er gaat niets naar een server.

> **Niet verbonden aan de Kiesraad.** Dit is een onafhankelijk hobbyproject. Het is niet gemaakt, gecontroleerd of goedgekeurd door de Kiesraad. Gebruik het alleen voor test en oefening, met fictieve gegevens. Controleer bestanden altijd tegen de officiële XSD's en modellen.

## Apps

| App | Bestand | EML |
|---|---|---|
| Verkiezingsdefinitie-maker | [`verkiezingsdefinitie.html`](verkiezingsdefinitie.html) | 110a |
| Stembureaus-maker | [`stembureaus.html`](stembureaus.html) | 110b (1.2.2 en 1.3.1) |
| Kandidatenlijsten-maker | [`kandidatenlijsten.html`](kandidatenlijsten.html) | 230b, 230c (totaallijsten met adressen), 210 (inlevering per lijst) |
| Uitslagen-generator | [`uitslagen-generator.html`](uitslagen-generator.html) | 510b, 510c, 510d (optioneel ondertekend) |
| PV-generator | [`pv-generator.html`](pv-generator.html) | leest 110a/b, 230b, 510b/c/d/r, 520; maakt PDF |
| Controleprotocol (GR26) | [`controleprotocol.html`](controleprotocol.html) | leest 110a/b, 230b, 510b/c/d; offline invoer via Excel |
| Controle zetelverdeling | [`zetelverdeling.html`](zetelverdeling.html) | leest 110a, 230b, 510b/c/d, 520 |

Wat er per versie is veranderd, staat in [`CHANGELOG.md`](CHANGELOG.md). Wat er nog op de to-do-lijst staat, staat in [`WENSEN.md`](WENSEN.md).

De startpagina is [`index.html`](index.html), de gebruikershandleidingen staan in [`handleidingen.html`](handleidingen.html). Technische notities en bekende beperkingen per app staan in [`docs/`](docs/).

De volgorde waarin je de apps meestal gebruikt: **110a → 110b → 230b → 510 → PV / controleprotocol → controle zetelverdeling.**

## Gebruiken

- **Online:** <https://bonzo261.github.io/EML-hulpmiddelen/>
- **Lokaal:** download de repository en open `index.html` in de browser. Laat de map `huisstijl/` naast de HTML-bestanden staan: daarin zitten het logo, de iconen en de lettertypen voor de PDF's van de PV-generator.

Externe afhankelijkheden, geladen vanaf een CDN (daarvoor is internet nodig):
- Google Fonts (IBM Plex)
- [SheetJS](https://sheetjs.com/) (xlsx) voor de Stembureaus-maker
- [pdfmake](http://pdfmake.org/) en [JSZip](https://stuk.github.io/jszip/) voor de PV-generator (van jsDelivr, met unpkg en cdnjs als reserve)

Sommige apps bewaren hun werk in `localStorage` van je browser. De Uitslagen-generator bewaart daar ook de testsleutels voor ondertekening. Die sleutels verlaten je browser alleen als je ze zelf exporteert. **Zet geëxporteerde sleutelbestanden (.json) nooit in deze repository.**

## Wensen

De belangrijkste punten op de to-do-lijst:
- kandidatenlijsten voor de Eerste Kamer per provincie;
- referenda en de TK-kieskring Bonaire;
- de nieuwe bestanden proef inlezen in Abacus, OSV en e-KS.

De volledige lijst staat in [`WENSEN.md`](WENSEN.md). Ideeën en fouten zijn welkom als [issue](https://github.com/Bonzo261/EML-hulpmiddelen/issues).

## Bronnen en dank

- De opbouw, teksten en opmaak van de processen-verbaal komen uit de Typst-sjablonen van [kiesraad/abacus](https://github.com/kiesraad/abacus) (`backend/templates`, EUPL-1.2).
- Het formaat van de 510-bestanden en de ondertekening volgen de testbestanden en de code van Abacus.
- De Controle zetelverdeling rekent volgens de tekst van de Kieswet en toetst de uitkomst aan een port van de zetelverdeling in Abacus (`backend/apportionment`), getest met alle eind-tot-eindtests daarvan.
- Het Controleprotocol volgt de controleprotocollen GR26 van de Kiesraad (november 2025).
- De indeling van Nederland (kieskringen, waterschappen, deelgemeenten, kiescolleges) en de opbouw van de 110a-, 210-, 230b- en 230c-bestanden volgen [e-KS](https://github.com/kiesraad/e-ks), het kandidaatstellingssysteem van de Kiesraad (EUPL-1.2), stand 23-11-2026.
- PDF-lettertypen in `huisstijl/pv-fonts.js`: DM Sans (aangepast door de Kiesraad) en Geist Mono, beide onder de SIL Open Font License 1.1, uit `kiesraad/abacus`.

## Licentie

[EUPL-1.2](LICENSE), dezelfde licentie als Abacus.
