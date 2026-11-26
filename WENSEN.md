# Wensen en to-do

Wat er nog op de lijst staat, per onderwerp. Bijgewerkt op 26-11-2026. Wat al klaar is, staat in [`CHANGELOG.md`](CHANGELOG.md); de bekende beperkingen per app staan in [`docs/`](docs/).

Heb je een idee of vind je een fout? Maak een [issue](https://github.com/Bonzo261/EML-hulpmiddelen/issues) aan.

## Nieuwe mogelijkheden

**Kandidatenlijsten**
- **Eerste Kamer per provincie.** Bij de Eerste Kamer is elke provincie en elk eiland een kieskring voor de kandidaatstelling. De app maakt nu nog één set lijsten. Eerst uitzoeken hoe de 230b eruitziet, te beginnen met het e-KS-voorbeeld 230b-ek27.
- **210 voor de Eerste Kamer per provincie.** De 210 kan nu alleen een lijst voor alle provincies samen maken; een lijst voor één of enkele provincies (zoals het e-KS-voorbeeld voor Utrecht) nog niet.
- Een gemachtigde ook bij kandidaten die in Nederland wonen (in de 230c).
- Afwijkende lijstnummers per kieskring.
- Instellingen bewaren, zodat je na herladen verder kunt.

**Verkiezingsdefinities**
- **TK-kieskring Bonaire** met de eilandsgemeenten Bonaire, Sint Eustatius en Saba (9001–9003).
- **Referenda en eilandsverkiezingen.** e-KS kent ook de categorieën NR, PR, LR, ER en IR (vermoedelijk nationaal, provinciaal en lokaal referendum, eilandsraad en eilandsreferendum). Nog niet bevestigd.
- Werk bewaren in de browser.

**Controle van de uitslag**
- Controleprotocol: de Excel van de Kiesraad direct inlezen (nu kan alleen plakken).
- Controleprotocol CSB: ook onderdeel A (data-analyse) en de controle of het protocol bij de gemeenten is uitgevoerd.

**Overig**
- Controle tegen de XSD van EML_NL in de apps zelf.
- Stembureaus-maker: de extra velden uit een ingelezen 110b (BAG-id, wijk en buurt, tellocatie en dergelijke) volwaardig ondersteunen.
- PV-generator: loting bij gelijke stand (nu kiest de app de eerste lijst en meldt dat).

## Testen en controleren

- **Proef inlezen in Abacus of OSV** van de nieuwe bestanden:
  - 110a voor Provinciale Staten, waterschappen, Eerste Kamer en kiescolleges;
  - 110a met DEELGEMEENTE (stadsdelen en gebieden) en met Fries;
  - 230b met geboortedatum, geslacht onbekend en kr:ListData;
  - 230c (totaallijsten met adressen) in OSV2020-KS of een ander programma dat 230c leest. Abacus leest geen 230c;
  - namen met bijzondere tekens (ş, ł, ễ enz.) uit de Kandidatenlijsten-maker: komen ze goed door Abacus, OSV en e-KS, ook op stembiljet en PV?
  - 210 (inlevering per lijst) in e-KS, zodra dat kan. Vooral bij gemeenteraad, stadsdelen en Europees Parlement, waar e-KS nog geen voorbeeld heeft.
- **Romeinse cijfers bij Limburg:** e-KS schrijft kieskring "1", Abacus verwacht "I" (bevestigd in de Abacus-code). Nagaan wat OSV verwacht, en of e-KS en Abacus hierin gelijk worden getrokken.
- **Met echte bestanden testen:** een 520 van de uitslagsoftware (en de 520 uit de Uitslagen-generator daarnaast leggen, ook in de PV-generator en de Controle zetelverdeling), en een 510d van de Tweede Kamer of het Europees Parlement met meerdere kieskringen.
- Uitslagen-generator: de 520 bij meerdere kieskringen (TK, PS) per kieskring volgens P 17–P 19 uitrekenen, en overleden kandidaten ondersteunen.
- **Ondertekening** van de Uitslagen-generator testen in een draaiende Abacus, en het certificaat voor HSB en CSB nalopen zodra Abacus dat vastlegt.
- Het Excel-bestand van het Controleprotocol testen in Microsoft Excel.
- Bevestigen dat de PV-generator op de website goed laadt.
- **Controle zetelverdeling:**
  - de berekening volgens de wettekst laten nakijken door iemand die de Kieswet kent;
  - de uitlegpunten (P 8 lid 3, P 9) voorleggen aan de Kiesraad.

## Gegevens bijwerken

- Het aantal statenzetels van tien provincies komt nog uit 2023.
- Waterschapszetels controleren bij de bekendmaking van de kandidaatstelling. Bij Delfland, Schieland en de Krimpenerwaard, Hollandse Delta en Noorderzijlvest komen ze nog uit een ontwerpbesluit.
- Kiescolleges: het aantal leden van het kiescollege niet-ingezetenen controleren na de registratie (sluit 3-2-2027), en nagaan of de kiescolleges in Caribisch Nederland echt evenveel leden hebben als de eilandsraad.
- Het landelijk register van partijnamen bijwerken (nu stand 27-08-2026).
- Nagaan of de 14 Rotterdamse gebieden uit e-KS nog kloppen; Rotterdam werkt sinds 2022 met wijkraden.
- Nagaan of de bijzondere regio 9010 (NBSB, Den Haag) in een PS-definitie hoort.
- PV-modellen N 5, O 7 en P 22-1 bijwerken zodra de Kiesraad of Abacus ze vastlegt.
- Wetsvoorstel 36 641 (drempel voor restzetels) volgen. Als het in werking treedt, moet de Controle zetelverdeling worden aangepast.
