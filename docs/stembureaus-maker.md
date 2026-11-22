# App: Stembureaus-maker (110b)

Bestand: `stembureaus.html`

**Doel:** de lijst met stembureaus van één gemeente maken als EML 110b, in de oude of de nieuwe standaard. Invoer op het scherm of uit Excel.

**Invoer:** een 110a (verkiezing overnemen), een bestaand 110b, een Excel/csv-bestand of geplakte cellen.

**Uitvoer:** `.eml.xml` met een SHA-256-hashcode, en de XML om te kopiëren. Een overzicht per stembureau (kaartjes) en een XML-weergave.

## Vragen
1. **Standaard**
   - EML_NL 1.2.2 (oud, gebruikt bij de herindelingsverkiezingen van 18-11-2026): nummer, naam en eventueel postcode.
   - EML_NL 1.3.1 (nieuw): ook straat, huisnummer, letter, toevoeging, postcode, plaats en type (vast, mobiel, bijzonder), zoals Abacus ze gebruikt (`kr:Schema`, `sb:Location`, `kr:ReportingUnitType`).
2. **Verkiezing en gemeente:** bij voorkeur overnemen uit de 110a, zodat Id, naam en datum gelijk zijn. Anders zelf invullen (soort, subcategorie, datum, naam, Id). Gemeente uit de keuzelijst (incl. fictieve gemeenten 0123–0126) of zelf.
3. **Stembureaus**, drie tabbladen:
   - **Invoerscherm:** toevoegen, sorteren op nummer, opnieuw nummeren, alles wissen, fictief voorbeeld.
   - **Uit Excel:** xlsx/xls/xlsm/ods/csv inlezen, of cellen plakken inclusief kopregel. Invulsjabloon (.xlsx) te downloaden met alleen de kolommen voor de gekozen standaard. Kolommen worden automatisch gekoppeld op naam (veel synoniemen), met keuze van werkblad en kopregel. Dubbele nummers worden gemeld. Daarna: vervangen, aanvullen op nummer, of toevoegen.
   - **Bestaand bestand:** een 110b inlezen (oud of nieuw), omzetten naar de nieuwe standaard, wijzigen op het invoerscherm of aanvullen met Excel.
4. **Bestand:** optioneel contactgegevens en verkiezingswebsite van de gemeente. Geavanceerd: ManagingAuthority Id en naam, TransactionId, IssueDate.

## Samenvatting rechts
Aantal stembureaus, mobiel/bijzonder, met adres, kiesgerechtigden; meldingen; download.

## Controles
- Dubbele stembureaunummers (fout), postcode in de vorm 1234 AB, huisnummer begint met een getal, huisletter één teken, BAG-id 16 cijfers, websites met http(s)://.
- Vaste stembureaus zonder volledig adres (straat, huisnummer, plaats) in de nieuwe standaard; mobiele stembureaus hoeven geen adres.
- Meerdere stembureaus op hetzelfde adres worden gemeld (in orde bij hetzelfde stemlokaal).

## Opslag
- Bewaard in localStorage (`stembureaus-maker-v1`).

## Open punten
- **XSD:** de app valideert niet tegen de XSD. Altijd eerst proef inlezen.
- **Ondersteuning 1.3.1:** nagaan of de software die het bestand inleest EML_NL 1.3.1 al ondersteunt (de app meldt dit bij elke download in de nieuwe standaard).
- **Extra velden uit een ingelezen 110b** (BAG-id, wijk en buurt met CBS-codes, gebruiksdoel gebouw, tellocatie, gedeelde locatie, website, extra adresaanduiding) blijven in het bestand staan, maar het invulsjabloon bevat alleen de kolommen die Abacus gebruikt. Bepalen of deze velden volwaardig ondersteund moeten worden.
- **Oude standaard:** adresgegevens die al zijn ingevuld, gaan in 1.2.2 verloren (alleen postcode blijft). De app meldt dit.
- **Excel-lezer** (SheetJS) wordt van cdnjs geladen. Lukt dat niet, dan alleen csv of plakken.
- **Kiesgerechtigden** mogen leeg blijven (leeg `PollingStation`-element). Nagaan of de ontvangende software dat accepteert.
