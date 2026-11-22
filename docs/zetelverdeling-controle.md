# App: Controle zetelverdeling (CSB, onderdeel B)

Bestand: `zetelverdeling.html`

Gebouwd en getest op 22-11-2026. Drie stappen als tabbladen.

## Wat het protocol vraagt
- Het CSB berekent de zetelverdeling zelf (met de hand, rekenmachine of spreadsheet van de Kiesraad): eerst de volle zetels, dan de restzetels, volgens Kieswet P 3 t/m P 10.
- In het Verslag controleprotocol CSB staat of de uitkomst gelijk is aan die van de programmatuur. Bij een verschil: nader onderzoek en een toelichting.
- Moment: na de rest van het protocol en vóór de vaststelling in de openbare zitting.

## Kieswet nagekeken (geldende tekst per 1-10-2026, wetten.overheid.nl)
- **P 2/P 3:** gelijkluidende lijsten en lijstengroepen tellen als één lijst. De app telt de TotalVotes van alle contests per AffiliationIdentifier op.
- **P 4 en P 11 zijn vervallen** (lijstencombinaties afgeschaft per 1-12-2017). Geen uitzonderingen meer.
- **P 5:** kiesdeler = som van de stemcijfers / aantal zetels. **P 6:** volle zetels = hoe vaak de kiesdeler in het stemcijfer past.
- **P 7 (≥ 19 zetels):** grootste gemiddelden (stemcijfer / (zetels + 1)), achtereenvolgens; bij gelijke gemiddelden zo nodig het lot. **Lid 2:** bij de Tweede Kamer geen restzetel voor lijsten onder de kiesdeler. Bij het EP geldt dat ook (blijkt uit het PV EP 2019). In de app: vinkje "kiesdelerdrempel", standaard aan voor TK en EP.
- **P 8 (< 19 zetels):** grootste overschotten, achtereenvolgens; lijsten zonder overschot gelden als het kleinste overschot; bij gelijkheid het lot. **Lid 2:** lijsten onder 75% van de kiesdeler doen niet mee. **Lid 3:** hebben alle lijsten die meedoen een restzetel en zijn er nog zetels over, dan grootste gemiddelden (P 7 lid 1), met hoogstens één zetel per lijst. De tekst zegt niet of lijsten onder 75% hier meedoen; Abacus laat ze wél meedoen en de app volgt dat.
- **P 9:** heeft een lijst de volstrekte meerderheid (> 50%) van de stemmen maar niet meer dan de helft van de zetels, dan krijgt zij alsnog één zetel; die vervalt bij de lijst met het kleinste gemiddelde/overschot (de laatst toegewezen restzetel); bij gelijkheid het lot.
- **P 10:** krijgt een lijst meer zetels dan kandidaten, dan gaan de overige zetels door voortgezette toepassing van de vorige bepalingen naar andere lijsten.
- Verschil per verkiezingssoort: alleen de kiesdelerdrempel (TK, EP). De Eerste Kamer gebruikt hoofdstuk U met stemwaarden; de app waarschuwt daarvoor.
- **Wetsvoorstel 36 641 (Wet aanscherping vereisten toewijzing restzetels):** zou de kiesdeler als drempel voor restzetels bij alle verkiezingen invoeren (behalve EK) en in P 8 lid 3 "hoogstens één zetel per lijst" schrappen. Stand 22-11-2026: niet aangenomen, in juni 2025 controversieel verklaard, motie tot intrekking (nov. 2026). Niet in werking; de app volgt de huidige tekst. Bij inwerkingtreding: P 7 lid 2 voor alle categorieën aanzetten en P 8 lid 2/3 aanpassen.

## Rekenregels in de app
- De rekenkern (`ZV.apportion`) is een port van de zetelverdeling in Abacus (github.com/kiesraad/abacus, crate `backend/apportionment`, `seat_assignment`). Exacte breuken (BigInt). Volgorde: volle zetels → restzetels → P 9 → P 10 (uitgeputte zetels eraf, daarna opnieuw toewijzen met uitgeputte lijsten uitgesloten).
- P 8 lid 3 zoals Abacus: "uniek grootste gemiddelde" over alle lijsten met stemmen (ook < 75%), elke lijst hoogstens één zo'n zetel; zijn alle lijsten al aan de beurt geweest, dan gewone grootste gemiddelden.
- Gelijke stand: krijgen alle lijsten met hetzelfde getal een zetel, dan geen loting. Anders vraagt de app de uitkomst van de loting van het CSB (knop per lijst). Lotingen worden op volgorde bewaard (`S.lots`), net als `lists_drawn` in Abacus. Past een loting niet meer bij gewijzigde gegevens, dan haalt de app hem weg.
- Uitbreiding t.o.v. Abacus: kiesdelerdrempel (P 7 lid 2) voor TK/EP. Abacus ondersteunt alleen GR en AB.

## Opbouw
1. **Gegevens**
   - EML: 510b/c/d (stemcijfers), 110a (kr:NumberOfSeats), 230b (namen en aantal kandidaten, uniek over contests), 520 (gekozen kandidaten per lijst, Elected = yes). SHA-256 per bestand.
   - Soort verkiezing (uit ElectionCategory) en het vinkje voor de kiesdelerdrempel.
   - Bewerkbare tabel: lijst, naam, stemcijfer, kandidaten. Plakken uit Excel (ook kolom C t/m H van de Kiesraad-sheet). Afwijkingen van de EML worden gemarkeerd en komen in het verslag.
   - Zonder aantal kandidaten wordt P 10 niet gecontroleerd (melding).
2. **Berekening**, stap voor stap
   - kiesdeler als breuk (bijv. 1.711 17/39);
   - volle zetels met quotiënt en overschot;
   - < 19: tabel met overschotten en 75%-drempel; ≥ 19 of P 8 lid 3: tabel met de gemiddelden per restzetel, het toegewezen vak gemarkeerd;
   - P 9 en P 10 apart toegelicht; lijst van alle stappen; CSV-export.
3. **Vergelijking en verslag**
   - Uitkomst van de programmatuur: 520 inlezen, of per lijst overtikken van het PV (totaal verplicht, vol/rest optioneel).
   - Oordeel gelijk/verschil; bij verschil is een toelichting nodig.
   - Kenmerken (hash, datum, tijd, controle eerste 8 tekens tegen de voettekst) voor 510 en 520.
   - Verslag "onderdeel B: controle zetelverdeling" als .html, met Ja/Nee, toelichting en bijzonderheden (loting, P 9, P 10, drempel, handmatige wijzigingen). Ook "Tekst kopiëren".
- Sessie in localStorage en als .json (`zetelverdeling/1`). Voorbeelden in het menu.

## Tests
- **Alle 46 eind-tot-eindtests uit Abacus** (`seat_assignment/mod.rs` en `residual_seat_assignment.rs`, inclusief lotingen, P 9 en P 10): goed.
- **'s-Hertogenbosch GR26:** 39 zetels, 66.746 stemmen, kiesdeler 1711 17/39, 31 vol + 8 rest, laagste gemiddelde 1414 1/8 (lijst 1): gelijk aan de ingevulde spreadsheet van de Kiesraad.
- 16 eigen gevallen (< 19 zetels met 75%-drempel, P 8 lid 3, gelijke stand met/zonder loting, P 9, P 10 met onbezette zetels, TK-drempel) en 16.712 willekeurige gevallen tegen een aparte eenvoudige berekening: geen verschil.
- In de browser (Chrome): alle voorbeelden, de EML-testbestanden van Abacus (510b/110a/230b) en een zelfgemaakte 520; smal scherm en donkere modus.

## Open punten
- Testen met een echte 520 van de programmatuur (de structuur Elected = yes is aangenomen) en met een echte 510d van TK/EP met meerdere contests.
- Abacus maakt (nog) geen 520; dan de uitkomst overtikken van het PV.
- De echte aantallen kandidaten van 's-Hertogenbosch zijn niet bekend; in het voorbeeld staat P 10 daarom op "niet gecontroleerd".
- EK (hoofdstuk U, stemwaarden) wordt niet apart ondersteund.
- De spreadsheet van de Kiesraad kan het volgens de Kiesraad zelf fout hebben bij P 9, P 10 en loting. De app ondersteunt die drie wél.
