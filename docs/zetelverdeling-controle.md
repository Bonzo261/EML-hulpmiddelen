# App: Controle zetelverdeling (CSB, onderdeel B)

Bestand: `zetelverdeling.html`

Gebouwd en getest op 22-11-2026; rekenkern volgens de wettekst sinds 23-11-2026. Drie stappen als tabbladen.

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

## Rekenregels in de app: twee kernen
**Waarom:** de controle moet onafhankelijk zijn van de programmatuur. Een port van Abacus zou fouten in de logica van Abacus overnemen. Daarom rekent de app volgens de wettekst; Abacus dient alleen als toetsing.

- **Hoofdberekening `ZVW.verdeel` (wet.js):** rechtstreeks uit de tekst van P 5 t/m P 10 geschreven, elke stap met de zin uit de wet erbij. Exacte breuken (BigInt).
  - P 10 als "voortgezette toepassing": een lijst zonder kandidaten zonder zetel doet niet meer mee. Volle zetels boven het aantal kandidaten worden extra restzetels.
  - P 8 lid 1: elke lijst hoogstens één restzetel via overschot ("achtereenvolgens"). Lid 3: grootste gemiddelden, hoogstens één per lijst.
  - P 9 na de verdeling: de laatst toegewezen restzetel van een andere lijst vervalt; bij gelijke waarde het lot.
- **Uitlegpunten, in de code gemarkeerd met UITLEG en in de app gemeld:**
  - `lid3`: doen lijsten onder 75% mee bij P 8 lid 3? Standaard ja (P 7 lid 1 kent geen drempel). Te wisselen in de app; de app meldt of het voor de uitkomst uitmaakt.
  - `lid3-op`: zijn er na P 8 lid 3 nog zetels over, dan regelt de wet dat niet; de app gaat door met P 7 lid 1 zonder beperking en meldt dat.
  - `p9-zelf`: had de meerderheidslijst zelf de kleinste restzetel, dan vervalt die van een andere lijst.
  - `p9-p10` en `p9-geen`: P 9 kan niet worden toegepast (geen kandidaten meer, of geen andere restzetel); melding.
- **Toetsing `ZV.apportion` (engine.js):** port van Abacus (github.com/kiesraad/abacus, `backend/apportionment`). De app rekent dezelfde gegevens na over alle mogelijke lotingen en meldt in stap 2, onderdeel 6: gelijk, verschil per lijst, of een loting die alleen Abacus vraagt.
- **Gevonden verschil in werkwijze:** bij een gelijke stand met een lijst die al haar kandidaten al heeft verkozen, vraagt Abacus om een loting en past daarna P 10 toe. Volgens de wettekst doet die lijst niet meer mee, dus is er geen loting nodig. De zetels komen in alle gevonden gevallen hetzelfde uit. Voorbeeld in de app: "Loting en lijstuitputting".
- Gelijke stand: de app vraagt de uitkomst van de loting van het CSB (`S.lots`, op volgorde) en meldt of die uitkomst voor de zetels uitmaakt. Bij P 9 kies je bij de loting de lijst waarvan de zetel vervalt. Met "Vorige loting ongedaan maken" haal je de laatste loting weg.

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
   - P 9 en P 10 apart toegelicht; onderdeel 6 "Toetsing van deze berekening"; lijst van alle stappen; CSV-export.
3. **Vergelijking en verslag**
   - Uitkomst van de programmatuur: 520 inlezen, of per lijst overtikken van het PV (totaal verplicht, vol/rest optioneel).
   - Oordeel gelijk/verschil; bij verschil is een toelichting nodig.
   - Kenmerken (hash, datum, tijd, controle eerste 8 tekens tegen de voettekst) voor 510 en 520.
   - Verslag "onderdeel B: controle zetelverdeling" als .html, met Ja/Nee, toelichting en bijzonderheden (loting, P 9, P 10, drempel, uitlegkeuzes, uitkomst van de toetsing, handmatige wijzigingen). Ook "Tekst kopiëren".
- Sessie in localStorage en als .json (`zetelverdeling/1`). Voorbeelden in het menu.

## Tests
- **Wetskern:** 's-Hertogenbosch GR26 goed (39 zetels, 66.746 stemmen, kiesdeler 1711 17/39, 31 vol + 8 rest, laagste gemiddelde 1414 1/8, gelijk aan de ingevulde spreadsheet van de Kiesraad). Dezelfde uitkomst als alle 46 eind-tot-eindtests uit Abacus (als feit gebruikt, niet als code). 160.000 willekeurige gevallen met P 9, P 10, P 8 lid 3 en lotingen (beide kernen met dezelfde lotkeuze): geen verschil in zetels. Gericht gezocht naar P 9 samen met P 10 (467 gevallen): geen verschil. Wel ongeveer 5.600 gevallen waarin alleen Abacus om een loting vraagt; de uitkomst van die loting maakt dan niet uit.
- **Abacus-port:** alle 46 Abacus-tests goed; 16 eigen gevallen; 16.712 willekeurige gevallen tegen een eenvoudige berekening.
- In de browser (Chrome): alle voorbeelden, de EML-testbestanden van Abacus (510b/110a/230b) en een zelfgemaakte 520; smal scherm en donkere modus.
- Kanttekening: de wetskern is geschreven door dezelfde AI-assistent die de Abacus-code al had gelezen. Verschillen worden daarom gemeld en niet weggewerkt; een review van wet.js door een mens naast de wettekst is aan te raden.

## Open punten
- Review van de wetskern (wet.js) door iemand die de Kieswet kent, vooral de uitlegpunten.
- De uitlegpunten (P 8 lid 3, P 9 bij P 8 lid 3) voorleggen aan de Kiesraad.
- Testen met een echte 520 van de programmatuur (de structuur Elected = yes is aangenomen) en met een echte 510d van TK/EP met meerdere contests.
- Abacus maakt (nog) geen 520; dan de uitkomst overtikken van het PV.
- De echte aantallen kandidaten van 's-Hertogenbosch zijn niet bekend; in het voorbeeld staat P 10 daarom op "niet gecontroleerd".
- EK (hoofdstuk U, stemwaarden) wordt niet apart ondersteund.
- De spreadsheet van de Kiesraad kan het volgens de Kiesraad zelf fout hebben bij P 9, P 10 en loting. De app ondersteunt die drie wél.
