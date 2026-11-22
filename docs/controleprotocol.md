# App: Controleprotocol (GSB · HSB · CSB)

Bestand: `controleprotocol.html`

**Doel:** de digitale vervanger van de controle-spreadsheet van de Kiesraad. De app controleert of de uitslagsoftware goed heeft opgeteld (art. Na 27 Kieswet).

**Uitgangspunt:** de controle-invoer loopt naast de invoer in Abacus. Je kunt de uitslag-EML daarom pas inlezen als alle controle-invoer compleet is. Zo kan er nooit eerder vergeleken worden.

## Stappen
1. **Gegevens**
   - Kies het niveau zelf: GSB, HSB of CSB.
   - Structuur uit 110b (stembureaus), 110a (gemeenten per kieskring of kieskringen) en 230b (lijsten).
   - Eenheden en lijsten kun je ook zelf plakken, bijvoorbeeld uit de Excel van de Kiesraad.
   - Een 510 wordt hier geweigerd.
2. **Loting**
   - Steekproef (standaard 3) of alle lijsten.
   - Loten uit de pot en aanklikken, of digitaal trekken (crypto.getRandomValues).
   - Wordt vastgelegd met datum en tijd.
3. **Invoer**
   - Een raster met de eenheden × de gecontroleerde lijsten.
   - Enter gaat per rij of per kolom verder.
   - In het voorbeeld is er een knop die de juiste getallen invult (eventueel met één afwijking).
4. **Uitslag inlezen en vergelijken**
   - Inlezen van de 510 (b/c/d, passend bij het niveau) kan alleen als de loting vastligt en alle vakken zijn ingevuld.
   - Daarna liggen stap 1 en de loting vast. Met "Uitslag weghalen" zet je ze weer open.
   - Vergelijking per lijst: de som van de invoer tegen TotalVotes.
   - Bij een verschil zie je per eenheid de afwijking tegen ReportingUnitVotes, en een knop "Alle lijsten controleren". De nieuwe lijsten moeten eerst helemaal ingevoerd zijn; tot die tijd blijven de cijfers uit de uitslag verborgen.
   - Controles op de uitslag-EML zelf:
     - komen de eenheden en lijsten overeen met de invoer;
     - is het dezelfde verkiezing;
     - tellen de eenheden op tot het totaal;
     - tellen de kandidaatstemmen op tot het lijsttotaal.
   - Verslag (.html) met de drie aankruisopties uit het Kiesraad-verslag, plus .xlsx (SheetJS via cdnjs) en CSV. Cijfers uit de uitslag staan pas in het verslag en de export als de vergelijking is toegestaan.

## Koppelen van ID's (`canonKey`)
- GSB: het laatste getal in het ID. Zo is `9999::SB3` gelijk aan PollingStation `3`.
- HSB: het deel na `::`, met 4 cijfers. Zo is `HSB1::0344` gelijk aan RegionNumber `344`.
- CSB: het laatste getal. Zo is `HSB2` gelijk aan kieskring `2`.

## Opslag
- De sessie wordt bewaard in localStorage en is te exporteren en importeren als .json.

## Open punten
- De Excel van de Kiesraad direct inlezen. Nu kan alleen plakken.
- Een geïmporteerde sessie kan een 510 bevatten terwijl de invoer nog niet compleet is. De vergelijking blijft dan wel verborgen tot alles is ingevoerd.
- 510d met meerdere contests is nog niet getest met een echt TK-bestand.
