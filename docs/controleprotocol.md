# App: Controleprotocol (GSB · HSB · CSB)

Bestand: `controleprotocol.html`

Vier stappen als tabbladen.

**Doel:** de digitale vervanger van de controle-spreadsheet van de Kiesraad. De app controleert of de uitslagsoftware goed heeft opgeteld (art. Na 27 Kieswet).

**Uitgangspunt:** de controle-invoer loopt naast de invoer in Abacus. Je kunt de uitslag-EML daarom pas inlezen als alle controle-invoer compleet is. Zo kan er nooit eerder vergeleken worden.

## Bronnen
**De app volgt alleen de controleprotocollen GR26 (Kiesraad, november 2025).** Die hebben de versie van 2023/2024 vervangen.
- **GSB:**
  - loting van 3 lijsten; spreadsheet vrij van opzet (stembureaus × Lijst A/B/C + Totaal);
  - invoer vanaf N 10-1 (DSO) of Na 31-2 bijlage 1 (CSO);
  - invoeren mag zodra het betreffende stembureau in de uitslagprogrammatuur staat;
  - vergelijken pas als alles in de programmatuur én in de spreadsheet staat, met rubriek 2.3 van Na 31-1/Na 31-2;
  - bij een verschil: eerst de invoer controleren (stap 5), anders direct telefonisch contact met de Kiesraad (stap 6). Uitbreiden naar alle lijsten staat niet meer in het protocol;
  - verantwoording in rubriek 2.5 van het PV; geen apart verslag.
- **CSB:**
  - onderdeel A: data-analyse, met het hulpmiddel op github.com/kiesraad/HCP;
  - onderdeel B: controle van de uitwisseling via Teluitslagen, of het controleprotocol GSB positief is uitgevoerd, de lijsttotalen tegen P 22, en de zetelverdeling (P 3 t/m P 10);
  - Verslag controleprotocol CSB.
- Voor het HSB is er geen versie GR26 (bij GR is er geen HSB). De app gebruikt neutrale teksten: Na 31 van de GSB's tegen O 7.
- Het oude protocol van 2023 (alleen ter achtergrond): https://www.kiesraad.nl/documenten/2023/10/17/controle-protocollen-vaststelling-verkiezingsuitslag

## Stappen
1. **Gegevens**
   - Kies het niveau zelf: GSB, HSB of CSB. Bij het GSB kies je ook de **stemopneming**: DSO of CSO.
   - Daarmee noemen alle schermen, het Excel en het verslag de juiste processen-verbaal en rubrieken (functie `pvInfo`).
   - Structuur uit 110b (stembureaus), 110a (gemeenten per kieskring of kieskringen) en 230b (lijsten).
   - **CSB: werkt de verkiezing met hoofdstembureaus?** (`hasHSB`). Regel: alleen bij **meer dan één kieskring**.
     - Ja, als de 110a meer dan één kieskring heeft, of zonder 110a bij categorie TK of EP. Dan zijn de eenheden de hoofdstembureaus en voer je de **O 7's van de HSB's** in, tegen de lijsttotalen in P 22-1.
     - Nee als de 110a één kieskring heeft (bijv. een provincie met één kieskring) of geen kieskringen, en zonder 110a bij categorie GR, AB (waterschap), EK, ER, BC of GC. Dan zijn de eenheden de gemeenten uit de 110a en voer je de Na 31-1/Na 31-2 van de GSB's in (met eventueel corrigendum), tegen P 22-1.
     - Onbekend, bijvoorbeeld PS zonder 110a: de tekst noemt beide.
   - Eenheden en lijsten kun je ook zelf plakken. Een 510 wordt hier geweigerd.
   - Hier kun je ook een Excel van de app slepen; de sessie wordt daaruit hersteld.
   - Na het vastleggen van de loting liggen de gegevens vast. Met "Loting opnieuw" zet je ze weer open.
2. **Loting**
   - Steekproef (standaard 3) of alle lijsten.
   - Loten uit de pot en aanklikken, of digitaal trekken (crypto.getRandomValues).
   - Wordt vastgelegd met datum en tijd, plus een **controlecode**. Advies: noteer die bij de loting.
   - **CSB en HSB met één gemeente: altijd alle lijsten, zonder loting** (`forceAll`).
3. **Invoer**
   - **Online:** een raster met de eenheden × de gecontroleerde lijsten.
   - **Offline:** via Excel, zie hieronder.
   - De lijsten staan altijd op volgorde van lijstnummer. De volgorde van trekken staat in de loting en het verslag.
4. **Uitslag inlezen en vergelijken**
   - Inlezen van de 510 kan alleen als de loting vastligt en alle vakken zijn ingevuld.
   - **Kenmerken:**
     - de app rekent de SHA-256 uit over de bytes van het bestand; die is gelijk aan de hash in de voettekst van het PV;
     - de app toont de eerste 8 tekens plus de volledige hash;
     - invoervelden voor datum, tijd en de eerste 8 tekens uit de voettekst; datum en tijd zijn vooraf ingevuld uit kr:CreationDateTime;
     - de app controleert of de ingetikte tekens gelijk zijn aan de hash.
   - **Uitslaggeschiedenis** (`S.resHistory`): elke nieuw ingelezen uitslag met een andere hash komt erbij, met per uitslag de uitkomst.
   - Vergelijking per lijst: de som van de invoer tegen TotalVotes; per eenheid de afwijkingen tegen ReportingUnitVotes.
   - Controles op de EML zelf: komen eenheden en lijsten overeen, is het dezelfde verkiezing, tellen de eenheden op tot het totaal, tellen de kandidaatstemmen op tot het lijsttotaal.
   - Bij een verschil: de tekst van stap 5 en 6 van het GR26-protocol. De knop "Alle lijsten controleren" is weggehaald; de code staat nog uitgeschakeld in `renderStep4`.
   - **Verslag:**
     - GSB: "Uitkomst controleprotocol gemeentelijk stembureau", met een tekst voor rubriek 2.5 van het PV;
     - CSB: onderdeel B, controle lijsttotalen (ja/nee plus toelichting). De zetelverdeling hoort bij de aparte app Controle zetelverdeling;
     - HSB: een verslag met aankruisopties;
     - altijd met de kenmerken (hash, datum, tijd) van het gecontroleerde document.
   - Export als .xlsx (eigen writer) en CSV.

## Offline invoer (Excel)
- Na de loting maakt de app een .xlsx, zonder externe bibliotheek: eigen OOXML-writer met een zip zonder compressie.
  - Tabblad **Invoer**:
    - controlecode in C5; kopregel op rij 8; de eenheden vanaf rij 9;
    - de gelote lijsten vanaf kolom C, op volgorde van lijstnummer;
    - een totaalregel met SUM-formules.
  - Alleen de gele vakken zijn ontgrendeld. Het blad is beveiligd zonder wachtwoord, met selectLockedCells.
  - Gegevensvalidatie: alleen hele getallen vanaf 0.
  - Tabblad **_sessie** (veryHidden, structuur vergrendeld): JSON van de hele opzet. Formaat `controleprotocol-excel/1`.
- **Inlezen** (ook deflate en sharedStrings):
  - de app rekent de code opnieuw uit en controleert C5, de koppen en de rijen;
  - vakken met tekst of een negatief getal: het bestand wordt geweigerd, met een lijst van de cellen;
  - zelfde code: de aantallen worden ingevuld;
  - lege sessie: de sessie wordt hersteld uit het Excel;
  - andere sessie: eerst bevestigen;
  - is de uitslag al ingelezen, dan wordt een andere loting geweigerd.
- Wijzigingen na het inlezen (`S.edits`) komen in het verslag.
- **Controlecode:** de eerste 4 bytes van SHA-256 over verkiezing, niveau, kieskring, orgaan, eenheden, lijsten en loting.
- Getest met openpyxl en LibreOffice, ook een bestand dat LibreOffice opnieuw heeft opgeslagen. Nog niet getest in Microsoft Excel.

## Koppelen van ID's (`canonKey`)
- GSB: het laatste getal. Zo is `9999::SB3` gelijk aan PollingStation `3`.
- HSB: het deel na `::`, met 4 cijfers. Zo is `HSB1::0344` gelijk aan RegionNumber `344`.
- CSB: het laatste getal. Zo is `HSB2` gelijk aan kieskring `2`, en gemeentecode 0796 gelijk aan `796`.

## Opslag
- De sessie wordt bewaard in localStorage, als .json en in het Excel.

## Open punten
- Testen in Microsoft Excel.
- Bij de Eerste Kamer zijn de "gemeenten" in de 110a mogelijk andere regio's. Nakijken met een echte 110a/510 voor EK.
- Bij GR26 heeft het CSB ook onderdeel A (data-analyse via kiesraad/HCP) en de controle of het GSB-protocol is uitgevoerd. Die zitten niet in deze app.
- De ReportingUnit-ID's in een 510d bij één kieskring of zonder kieskringen (eenheden = gemeenten) zijn nog niet tegen een echt bestand gecontroleerd.
- 510d met meerdere contests is nog niet getest met een echt TK-bestand.
