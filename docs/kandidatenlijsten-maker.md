# App: Kandidatenlijsten-maker (230b)

Bestand: `kandidatenlijsten.html`

**Doel:** kandidatenlijsten (EML 230b) met fictieve personen maken voor test en oefening, per kieskring, met hashcodes.

**Invoer:** een 110a uit de Verkiezingsdefinitie-maker of de uitslagsoftware. Voorbeelden: gemeenteraad en Tweede Kamer.

**Uitvoer:** een zip met de 230b-bestanden, per bestand een hashcode-txt en een `Hashcodes_overzicht.txt`. Altijd een totaalbestand met alle kieskringen (nodig voor de uitslagvaststelling door het CSB), optioneel ook één bestand per kieskring (voor de HSB's).

**Referentie:** de 230b-voorbeelden van [e-KS](https://github.com/kiesraad/e-ks) (`src/models/eml/testdata/230b-ws27.eml.xml` en `230b-ps27-2.eml.xml`). Sinds 23-11-2026 volgt de app deze opbouw.

## Vragen
1. **Verkiezingsdefinitie inladen** (110a). De rest is vergrendeld tot dit gebeurd is.
2. **Aantal partijen.** De eerste krijgen de geregistreerde aanduidingen uit de 110a, de rest een fictieve naam. Per lijst aan te passen: aanduiding (leeg = blanco lijst), aantal kandidaten, en per kieskring:
   - **gelijk**: overal dezelfde lijst (stel gelijkluidende lijsten);
   - **regionale stellen**: zelfde top en lijstduwer, regionale kandidaten per groep kieskringen;
   - **elke kieskring anders**: lijstengroep;
   - **alleen enkele kieskringen**: de partij doet niet overal mee (bijv. `1,3-5`).
3. **Personen:** aandeel vrouwen (%, geldt voor kandidaten met bekend geslacht), aandeel geslacht onbekend (%, standaard 0), startgetal (zelfde startgetal = zelfde personen), knop `Nieuwe personen`, **geboortedatum opnemen** (standaard aan; 18 t/m 80 jaar op de dag van stemming), optioneel enkele kandidaten in het buitenland (bij alle soorten verkiezingen).
4. **Bestanden:** apart bestand per kieskring, `kr:ListData` opnemen (EML_NL 1.3+) met **Geslacht publiceren** (PublishGender) en **Taal van publicatie** (`nl`, of `fy` als de 110a ergens `FrysianExportAllowed="true"` heeft). Bij kieskringen met Romeinse cijfers: schakelaar **Romeinse cijfers in ContestIdentifier** (standaard uit). Geavanceerd: ManagingAuthority Id en naam, TransactionId, IssueDate.

## Resultaat rechts
- Kieskringen, lijsten, unieke kandidaten, aantal bestanden.
- Weergave als stembiljet (per kieskring; rood nummer = regionale kandidaat; zonder geboortedatum; bij geslacht onbekend geen geslachtsletter), een totaaloverzicht van alle kieskringen (S = stel, G = lijstengroep, Z = op zichzelf staand), of als XML.

## Opbouw van een kandidaat (zoals e-KS)
`CandidateIdentifier` → `CandidateFullName` (NameLine Initials, FirstName, NamePrefix, LastName) → `DateOfBirth` (JJJJ-MM-DD) → `Gender` (`male`, `female` of `unknown`) → `QualifyingAddress` (Locality, of Country met CountryNameCode bij buitenland).
Per lijst: `<kr:ListData PublishGender="true|false" PublicationLanguage="nl|fy" [BelongsToSet="n"]/>`.

## Regels
- Maximaal 50 kandidaten per lijst, 80 voor partijen met meer dan 15 zetels.
- ManagingAuthority Id alleen CSB, HSB+nummer, SB+nummer of een 4-cijferige gemeentecode. Standaard `CSB` met "Centraal stembureau <naam>" (bij een gemeenteraad de gemeentecode).
- ElectionDomain krijgt alleen een Id als de 110a er een heeft.
- ContestIdentifier: `geen` bij één kieskring; anders het kieskringnummer met ContestName, ook bij Romeinse cijfers (zoals e-KS, 230b-ps27-2).
- Hashcode is SHA-256 over het hele bestand, in groepen van vier zoals op de PV's.
- Geboortedatum en geslacht onbekend komen uit eigen afgeleide random-generatoren (`seed|dob|sleutel`, `seed|gender|sleutel`). Namen, geslacht en woonplaatsen blijven daardoor gelijk aan eerdere versies met hetzelfde startgetal.
- Woonplaatsen zonder gemeenten in de 110a: PS-kieskring → TK-kieskring volgens de PROVINCIAAL_KIESKRING-nummers in de MasterElectionTree van e-KS (Gelderland 1 Arnhem, 2 Nijmegen; Noord-Holland 1 Amsterdam, 2 Den Helder, 3 Haarlem; Noord-Brabant 1 's-Hertogenbosch, 2 Tilburg).
- Alle personen zijn fictief; alleen voor test en oefening.

## Open punten
- **Romeinse cijfers: e-KS en Abacus verschillen.** e-KS schrijft bij Limburg `ContestIdentifier Id="1"`; Abacus verwacht bij een kieskring met `RomanNumerals="true"` het Id `I` (functie `as_contest_id` in de Abacus-code). Standaard volgt de app e-KS; zet de schakelaar aan voor Abacus.
- **Eerste Kamer:** de app maakt bij EK nog één set lijsten, terwijl elke provincie en elk eiland een kieskring voor de kandidaatstelling is.
- **XSD:** de app valideert niet tegen de XSD. Altijd eerst proef inlezen.
- **Abacus leest per bestand één kieskring.** Het totaalbestand met alle kieskringen is daar niet voor bedoeld; gebruik in Abacus de bestanden per kieskring.
- **Lijstnummers zijn in alle kieskringen gelijk.** Een afwijkende nummering per kieskring kan niet.
- **Lijsten met minder dan 4 kandidaten** worden altijd als ‘gelijk’ gemaakt, ook als regionale stellen of lijstengroep is gekozen. De app meldt dit niet.
- **Instellingen worden niet bewaard.** Bij elke start krijgt het startgetal een willekeurige waarde; noteer het startgetal om dezelfde personen terug te krijgen. Vul het startgetal in vóór het inladen van de definitie, anders verschilt de standaard partijtabel.
- **Woonplaatsen** komen uit de gemeenten in de 110a, of uit een vaste lijst per TK-kieskring. Ze zijn indicatief.
- **Grenzen:** maximaal 60 partijen; de XML-weergave toont maximaal 2500 regels (de download is volledig).
