# App: PV-generator

Bestand: `pv-generator.html` (met `huisstijl/pv-fonts.js`)

De app zelf gebruikt IBM Plex; de PDF volgt de opmaak van Abacus (DM Sans en Geist Mono).

**Doel:** processen-verbaal als PDF maken in de opmaak van Abacus, gevuld met de EML-bestanden uit de andere apps.

**Invoer:** 110a, 110b, 230b (uit de makers) en 510b, 510c, 510d, 510r, 520 (uit Abacus of de Uitslagen-generator). Het soort bestand wordt automatisch herkend. Voorbeelden: gemeenteraad en Provinciale Staten (fictieve gemeente Juinen).

**Uitvoer:** een PDF, of bij modellen per stembureau een zip met alle stembureaus.

## Modellen
**Stembureau** (per stembureau):
- N 10-1 Proces-verbaal van een stembureau (DSO) en N 10-1 Inlegvel (controles en correcties)
- N 10-2 Proces-verbaal van een stembureau (CSO)
- Na 31-2 Bijlage 1 Verslag telling stembureau (CSO)
- Na 14-1 versie 1 en versie 2 Corrigendum stembureau (eerste / nieuwe zitting, DSO)
- Na 14-2 Bijlage 1 Gecorrigeerde telresultaten stembureau (CSO)

**Gemeentelijk stembureau:**
- Na 31-1 (DSO) en Na 31-2 (CSO), met de inlegvellen voor controles in opdracht van het CSB
- Na 14-2 Corrigendum gemeentelijk stembureau
- P 2a Verslag nieuwe zitting
- N 5 Overdrachtsdocument (*)

**Hoofdstembureau:** O 7 Proces-verbaal van een hoofdstembureau (*), voor PS, TK en EP.

**Centraal stembureau:**
- P 22-2 Uitslag en zetelverdeling, en P 22-2 Bijlage 1 (stemmen per lijst en kandidaat), voor GR en waterschap
- P 22-1 Uitslag (TK, EP, PS) (*)

(*) Abacus maakt dit model niet. De opbouw volgt de nieuwe Kiesraad-modellen (N 5 versie 2026, concept PV hoofdstembureau 2024, P 22-1 2025) in de opmaak van Abacus; bedoeld om te testen.

## Vragen
1. **Welk proces-verbaal?** Filter op Alle / Decentraal (DSO) / Centraal (CSO), en optioneel `Alleen modellen die ik kan vullen`. Groene stip = alle benodigde EML's geladen, oranje = een deel. Modellen die niet bij de geladen soort verkiezing horen staan onder ‘Andere soorten verkiezing’.
2. **Welke EML-bestanden zijn nodig?** Per model een checklist; bestanden slepen of kiezen.
3. **Gegevens voor het proces-verbaal:** per model, bijv. stembureau, datum van de zitting, aanleiding van het onderzoek, kieskring.

## Testmodus
- Vult het PV met telresultaten uit de 510b (vakjes, kandidaatstemmen, totalen, selectievakjes), om de invoer in Abacus te testen.
- Bij corrigenda: ‘oorspronkelijk’ uit de 510b, ‘gecorrigeerd’ uit vorige waarden in de 510b, een 510r van de hertelling, of anders een fictieve correctie (2 stemmen naar een andere lijst).
- Zonder 510d gebruikt de app een 510c of de opgetelde 510b's.
- De PDF krijgt het watermerk TESTGEGEVENS.

## Controles
- Ontbrekende bestanden, bestanden van verschillende verkiezingen, model dat niet bij de soort verkiezing hoort, lijstnummers in de 510b die niet in de 230b staan.
- P 22-2: zetelverdeling berekend volgens Kieswet P 5–P 10 en vergeleken met de 520 (zetels per lijst en gekozen kandidaten).

## Resultaat rechts
- Model, aantal pagina's, lijsten, stembureaus; meldingen.
- `Download PDF`, `Alle stembureaus als zip`, `Opnieuw opmaken`.
- Inhoudsopgave van het PV met paginanummers.

## Bronnen en techniek
- Opbouw, teksten en opmaak uit de Typst-sjablonen van Abacus (`backend/templates`, versie 2027; EUPL-1.2).
- Leest EML_NL 1.2.2 en 1.3.x. PDF wordt in de browser gemaakt (pdfmake 0.2.20, JSZip 3.10.1).
- De bibliotheken worden geladen van jsDelivr, met unpkg en cdnjs als reserve. Lukt dat niet of loopt het opmaken vast, dan toont de app na hooguit 90 seconden een foutmelding in plaats van te blijven draaien.

## Opgeloste problemen
- **22-11-2026: PDF bleef draaien in productie.** Met de bestanden van ’s-Hertogenbosch (110a, 110b, 230b, 510b) werkten alle modellen lokaal wel. Oorzaak vermoedelijk het laden van pdfmake van cdnjs (versie daar niet beschikbaar) en fouten in de bibliotheek die niet werden gemeld. Opgelost met reserve-CDN's, een tijdslimiet en een foutmelding.

## Open punten
- **Hulpmiddel:** een PV uit de app altijd controleren tegen het vastgestelde model van de Kiesraad.
- **N 5, O 7 en P 22-1** maakt Abacus nog niet. Ze volgen (concept)modellen van de Kiesraad; bijwerken zodra Abacus of de Kiesraad ze vastlegt.
- **Loting** (restzetel tussen gelijke lijsten, Kieswet P 9, of tussen kandidaten met gelijke stemmen) kan de app niet doen. Ze kiest de eerste lijst of kandidaat en meldt dit; de zetelverdeling moet na de loting met de hand worden aangepast.
- **Gekozen kandidaten** kunnen afwijken van de 520, bijvoorbeeld door overleden kandidaten of loting. De app houdt daar geen rekening mee.
- **Onverdeelde zetels** worden alleen gemeld.
- **P 22-2** is alleen voor gemeenteraad en waterschap; voor PS, TK en EP is er P 22-1.
- **Fictieve correctie:** zonder 510r of vorige waarden maakt de testmodus een verzonnen correctie. Alleen voor testen.
- **Productie nog niet bevestigd:** de oplossing van 22-11-2026 is getest met nagebootste CDN's, niet in de echte productieomgeving.
