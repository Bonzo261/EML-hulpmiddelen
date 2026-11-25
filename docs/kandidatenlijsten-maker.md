# App: Kandidatenlijsten-maker (210, 230b en 230c)

Bestand: `kandidatenlijsten.html`

**Doel:** kandidatenlijsten (EML 230b), totaallijsten met adressen (EML 230c) en inleveringsbestanden per lijst (EML 210) met fictieve personen maken voor test en oefening, per kieskring, met hashcodes. De 210 is bedoeld om de inleverflow van e-KS te testen met losse kandidatenlijsten.

**Invoer:** een 110a uit de Verkiezingsdefinitie-maker of de uitslagsoftware. Voorbeelden: gemeenteraad en Tweede Kamer.

**Uitvoer:** een zip met per bestand een hashcode-txt en een `Hashcodes_overzicht.txt`:
- **230c** `Totaallijsten_<verkiezing>.eml.xml` (standaard aan): alle kieskringen in één bestand, met adressen. Dit is het totaalbestand voor de uitslagvaststelling door het CSB.
- **230b per kieskring** (standaard aan): voor de HSB's en voor Abacus. Bij één kieskring is er één 230b.
- **230b met alle kieskringen** (standaard uit, alleen bij meer kieskringen): het oude totaalbestand, zonder adressen.
- **210** (standaard uit): in de map `210_inlevering/`, één bestand per ingeleverde lijst: `Inlevering_<verkiezing>_lijst_<nr>_<partij>[_kieskring_<n>].eml.xml`.

**Referentie:** de voorbeelden van [e-KS](https://github.com/kiesraad/e-ks) (`src/models/eml/testdata/`): 210-ws27, -ps27-1, -ps27-2, -ek27; 230b-ws27, 230b-ps27-2; 230c-ws27, -ps27-1, -ps27-2, -ek27. Sinds 23-11-2026 (230b/230c) en 25-11-2026 (210) volgt de app deze opbouw.

## Vragen
1. **Verkiezingsdefinitie inladen** (110a). De rest is vergrendeld tot dit gebeurd is.
2. **Aantal partijen.** De eerste krijgen de geregistreerde aanduidingen uit de 110a, de rest een fictieve naam. Per lijst aan te passen: aanduiding (leeg = blanco lijst), aantal kandidaten, en per kieskring:
   - **gelijk**: overal dezelfde lijst (stel gelijkluidende lijsten);
   - **regionale stellen**: zelfde top en lijstduwer, regionale kandidaten per groep kieskringen;
   - **elke kieskring anders**: lijstengroep;
   - **alleen enkele kieskringen**: de partij doet niet overal mee (bijv. `1,3-5`).
3. **Personen:** aandeel vrouwen (%, geldt voor kandidaten met bekend geslacht), aandeel geslacht onbekend (%, standaard 0), startgetal (zelfde startgetal = zelfde personen), knop `Nieuwe personen`, **geboortedatum opnemen** (standaard aan; 18 t/m 80 jaar op de dag van stemming), optioneel enkele kandidaten in het buitenland (bij alle soorten verkiezingen).
4. **Bestanden:** 230c maken, 230b per kieskring, 230b met alle kieskringen, **210 maken** (met de opties BSN opnemen, standaard aan, en het aantal plaatsvervangers van de inleveraar, 1 of 2, standaard 2). `kr:ListData` opnemen (EML_NL 1.3+) met **Geslacht publiceren** (PublishGender) en **Taal van publicatie** (`nl`, of `fy` als de 110a ergens `FrysianExportAllowed="true"` heeft). Bij kieskringen met Romeinse cijfers: schakelaar **Romeinse cijfers in ContestIdentifier** (standaard uit). Geavanceerd: ManagingAuthority Id en naam, TransactionId, IssueDate.

## Resultaat rechts
- Kieskringen, lijsten, unieke kandidaten, aantal bestanden; per bestand de soort (210/230b/230c) en de hashcode.
- Weergave als stembiljet (per kieskring; rood nummer = regionale kandidaat; zonder geboortedatum; bij geslacht onbekend geen geslachtsletter), een totaaloverzicht van alle kieskringen (S = stel, G = lijstengroep, Z = op zichzelf staand), of als XML.
- Bij één kieskring kun je kiezen tussen de 230b en de 230c. In de 230c-weergave staat per kandidaat het correspondentieadres of de gemachtigde.
- In de keuzelijst staan ook de 210-bestanden ("210 – inlevering per lijst"). Per 210 zie je de ContestIdentifier, de kieskringen, de kandidaten met adres en BSN, en de inleveraar met plaatsvervangers.

## Opbouw van een kandidaat (zoals e-KS)
230b: `CandidateIdentifier` → `CandidateFullName` (NameLine Initials, FirstName, NamePrefix, LastName) → `DateOfBirth` (JJJJ-MM-DD) → `Gender` (`male`, `female` of `unknown`) → `QualifyingAddress` (Locality, of Country met CountryNameCode bij buitenland).
Per lijst: `<kr:ListData PublishGender="true|false" PublicationLanguage="nl|fy" [BelongsToSet="n"]/>`.

230c, bovenop de 230b:
- eerste keer dat een persoon voorkomt: `<CandidateIdentifier Id="n" ShortCode="…"/>`. De korte code bestaat uit de letters van de achternaam plus de voorletters, zonder voorvoegsel. Hij is maximaal 15 bytes, uniek in het bestand, en wordt zo nodig genummerd (`JansenJ`, `JansenJ2`);
- kandidaat in Nederland: `Contact/MailingAddress/xal:Locality` met `xal:AddressLine`, `xal:LocalityName` en `xal:PostalCode/xal:PostalCodeNumber` (bijv. `1234AB`);
- kandidaat in het buitenland: geen eigen Contact, maar `Agent Role="H10"` (gemachtigde) met naam, Contact en `kr:LivingAddress/kr:LocalityName`;
- dezelfde persoon van dezelfde partij in een volgende kieskring: alleen `<CandidateIdentifier Id="n"><ShortCode>…</ShortCode></CandidateIdentifier>`.

210 (één bestand per ingeleverde lijst):
- een partij met dezelfde lijst in alle kieskringen levert één 210 in; bij regionale stellen één per groep kieskringen; bij "elke kieskring anders" één per kieskring;
- `ManagingAuthority`: `<AuthorityIdentifier Id="0000"/>`, `<AuthorityAddress/>`, `<kr:CreatedByAuthority Id="0000">De politieke partij</kr:CreatedByAuthority>`;
- `Nomination` → `ElectionIdentifier` (zoals de 110a) → `ContestIdentifier` → `Affiliation` → `Nominate`;
- ContestIdentifier (altijd met ContestName): `geen` met lege naam bij één kieskring; `alle` met lege naam als de lijst voor alle kieskringen geldt; anders nummer en naam van de eerste kieskring van de lijst;
- Affiliation: AffiliationIdentifier zonder Id, met RegisteredName; Type altijd `op zichzelf staande lijst` (zoals e-KS); `kr:ListData` met PublishGender, PublicationLanguage en `kr:Contests` (een `kr:Contest Id="n"` per kieskring van de lijst);
- kandidaat: als in de 230c maar zonder korte code, gevolgd door `kr:NationalIdentificationNumber` (BSN);
- Nominate: de inleveraar (`inleveraar`, zonder Id) en 1 of 2 plaatsvervangers (`plaatsvervanger van de inleveraar`, Id 1, 2), elk met voorletters, achternaam en correspondentieadres. Per partij dezelfde indieners in al haar 210's.

Getest (210) met 110a-ws27, -ps27-1, -ps27-2, -ek27 en de GR- en TK-testdefinities van Abacus: opbouw zoals 210-ps27-2; ContestIdentifier en kr:Contests gelijk aan de e-KS-voorbeelden; alle BSN's halen de elfproef en horen elk bij één persoon; met de 210 uit zijn de 230b- en 230c-bestanden byte voor byte gelijk aan de vorige versie.

Getest (230c) met 110a-ws27, 110a-ps27-2, 110a-ek27 en de TK-testdefinitie van Abacus: korte codes uniek en geldig, alle verwijzingen bekend; de opbouw van een kandidaat (Nederland, buitenland, verwijzing) is gelijk aan 230c-ws27 en 230c-ps27-2; de 230b-bestanden zijn byte voor byte gelijk aan de vorige versie (afgezien van CreationDateTime).

## Regels
- Maximaal 50 kandidaten per lijst, 80 voor partijen met meer dan 15 zetels.
- ManagingAuthority Id (230b/230c) alleen CSB, HSB+nummer, SB+nummer of een 4-cijferige gemeentecode. Standaard `CSB` met "Centraal stembureau <naam>" (bij een gemeenteraad de gemeentecode). In de 210 altijd `0000` (de politieke partij).
- ElectionDomain krijgt alleen een Id als de 110a er een heeft.
- ContestIdentifier: `geen` bij één kieskring; anders het kieskringnummer met ContestName, ook bij Romeinse cijfers (zoals e-KS, 230b-ps27-2).
- Hashcode is SHA-256 over het hele bestand, in groepen van vier zoals op de PV's.
- BSN: fictief, uit de reeks 999990000-999999999 die de elfproef haalt (909 nummers; ook het e-KS-voorbeeld 999995972 valt hierin). Elke persoon houdt in alle bestanden hetzelfde BSN. Meer dan 909 kandidaten geeft een waarschuwing.
- Geboortedatum, geslacht onbekend, adressen/gemachtigden, BSN's en indieners komen uit eigen afgeleide random-generatoren (`seed|dob|sleutel`, `seed|gender|sleutel`, `seed|adres|sleutel`, `seed|bsn`, `seed|inleveraar|lijstnummer`). Namen, geslacht en woonplaatsen blijven daardoor gelijk aan eerdere versies met hetzelfde startgetal; de 230b- en 230c-bestanden zijn door de 210 niet veranderd.
- Woonplaatsen zonder gemeenten in de 110a: PS-kieskring → TK-kieskring volgens de PROVINCIAAL_KIESKRING-nummers in de MasterElectionTree van e-KS (Gelderland 1 Arnhem, 2 Nijmegen; Noord-Holland 1 Amsterdam, 2 Den Helder, 3 Haarlem; Noord-Brabant 1 's-Hertogenbosch, 2 Tilburg).
- Alle personen, adressen en BSN's zijn fictief; alleen voor test en oefening. Straatnamen en postcodes zijn willekeurig en kunnen toevallig bestaan.

## Open punten
- **210 proef inlezen in e-KS** zodra dat kan.
- **210 bij GR, BC/GC en EP:** e-KS heeft daar geen voorbeeld-210. De app zet in kr:Contests het nummer 1 met de naam van het gebied. Controleren zodra e-KS die verkiezingen ondersteunt.
- **210 bij EK:** de app maakt één lijst voor alle provincies (`alle`, met de 16 provincies in kr:Contests). Een lijst voor één of enkele provincies, zoals 210-ek27 (7 Utrecht), kan nog niet.
- **Lijsttype in de 210** is altijd `op zichzelf staande lijst`, ook bij regionale stellen of een lijstengroep, omdat e-KS het zo vastzet. In de 230b/230c staat het echte type.
- **Romeinse cijfers: e-KS en Abacus verschillen.** e-KS schrijft bij Limburg `ContestIdentifier Id="1"`; Abacus verwacht bij een kieskring met `RomanNumerals="true"` het Id `I` (bevestigd in de Abacus-code, functie `as_contest_id`; het PS2-testbestand van Abacus gebruikt ook `II`). Standaard volgt de app e-KS; zet de schakelaar aan voor Abacus. In de 210 gebruikt de app altijd het nummer, zoals e-KS.
- **230c proef inlezen:** Abacus leest geen 230c. Proef inlezen in OSV2020-KS of een ander programma dat 230c leest.
- **Gemachtigde alleen bij buitenland.** e-KS geeft alleen een kandidaat in het buitenland een gemachtigde (H 10). Een gemachtigde bij een kandidaat in Nederland maakt de app niet.
- **Eerste Kamer:** de app maakt bij EK nog één set lijsten, terwijl elke provincie en elk eiland een kieskring voor de kandidaatstelling is.
- **XSD:** de app valideert niet tegen de XSD. Altijd eerst proef inlezen.
- **Abacus leest per bestand één kieskring.** Gebruik in Abacus de 230b per kieskring.
- **Lijstnummers zijn in alle kieskringen gelijk.** Een afwijkende nummering per kieskring kan niet.
- **Lijsten met minder dan 4 kandidaten** worden altijd als ‘gelijk’ gemaakt, ook als regionale stellen of lijstengroep is gekozen. De app meldt dit niet.
- **Instellingen worden niet bewaard.** Bij elke start krijgt het startgetal een willekeurige waarde; noteer het startgetal om dezelfde personen terug te krijgen. Vul het startgetal in vóór het inladen van de definitie, anders verschilt de standaard partijtabel.
- **Woonplaatsen** komen uit de gemeenten in de 110a, of uit een vaste lijst per TK-kieskring. Ze zijn indicatief.
- **Grenzen:** maximaal 60 partijen; de XML-weergave toont maximaal 2500 regels (de download is volledig).
