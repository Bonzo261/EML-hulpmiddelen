# App: Verkiezingsdefinitie-maker (110a)

Bestand: `verkiezingsdefinitie.html`

**Doel:** een verkiezingsdefinitie (EML 110a) maken die je in de uitslagsoftware kunt inlezen. Het bestand is de basis voor alle andere apps (stembureaus, kandidatenlijsten, PV's, uitslagen).

**Invoer:** de vragen op het scherm, of een bestaand 110a-bestand (`Bestaand 110a-bestand openen`).

**Uitvoer:** `.eml.xml` met een SHA-256-hashcode, en de XML om te kopiëren.

**Referentie:** de 110a-voorbeelden en de `MasterElectionTree.xml` van [e-KS](https://github.com/kiesraad/e-ks), het nieuwe kandidaatstellingssysteem van de Kiesraad (EUPL-1.2, stand 23-11-2026).

## Vragen
1. **Soort verkiezing:** GR, PS, AB (waterschap), TK, EP, EK, Kiescollege (vanaf EML_NL 1.3), stadsdeelcommissie (BC), wijkraad/gebiedscommissie (GC) en bestuursraad (BC; EML_NL kent geen aparte categorie).
2. **Gebied:** gemeente, provincie of waterschap uit een keuzelijst, of zelf invullen. Gemeentecode = 4-cijferige CBS-code met voorloopnullen. Provincienummers 1 (Groningen) t/m 12 (Limburg). Waterschappen: de 21 echte waterschappen uit e-KS (nummers 1–25; 4, 6, 21 en 22 ontbreken na fusies), standaard waterschap 1. Kiescolleges: 13 Bonaire, 14 Sint Eustatius, 15 Saba (KCCN) en 16 niet-ingezetenen (KCNI). Bij BC/GC/BR ook de naam van de commissie (keuzelijst Amsterdam, Rotterdam en 's-Hertogenbosch).
3. **Datums:** dag van stemming; dag van kandidaatstelling automatisch de 44e dag ervoor, bij de Eerste Kamer de 34e (aan te passen). Standaard EK: 24-05-2027.
4. **Zetels:** aantal zetels, voorkeurdrempel en subcategorie worden automatisch bepaald. Voorkeurdrempel: 25% van de kiesdeler, 50% bij minder dan 19 zetels, 100% voor de EK (Kieswet sinds Stb. 2010, 790) en 10% voor het EP. PS met één kieskring = PS1, meer kieskringen = PS2. Bij echte waterschappen wordt het aantal gekozen zetels ingevuld uit de provinciale reglementen 2027 (omvang algemeen bestuur min 4 geborgde zetels).
5. **Geregistreerde aanduidingen:** letterlijk uit het register van het CSB, één per regel. Knop voor het landelijk register (37 aanduidingen, stand 27-08-2026). Inlezen uit 110a, 230b, txt of csv; aanvullen of vervangen.
6. **Indeling (verkiezingsboom):** gebieden met categorie, nummer, naam, bovenliggend gebied, stembureaus (CSB/HSB/PROV_SB). Opties:
   - HSB opnemen bij het CSB zoals de Kiesraad-bestanden doen (de Kieswet kent alleen een HSB bij TK, EP en PS met meerdere kieskringen);
   - fictieve testindeling (Heemdamseburg, Juinen, Middelgein, 's-Gravenveen, Oost-Holland, Zuid-Brabant, Rivier en Polder, uit de Abacus-testbestanden);
   - gemeenten opnemen in de indeling (ook bij elk gekozen waterschap);
   - stadsdeel/gebied als DEELGEMEENTE opnemen (BC/GC met een nummer in e-KS; standaard uit);
   - gemeenten in één keer plakken als `code;naam;kieskringnummer`.
7. **Bestand:** EML_NL-versie 1.3.1 (huidig), 1.3 of 1.2.2 (herindelingsverkiezingen 18-11-2026); naam en Id automatisch. Geavanceerd: TransactionId, IssueDate, ManagingAuthority Id, MaxVotes, ContestIdentifier.

## Provinciale Staten (PS)
Opbouw zoals de e-KS-voorbeelden `110a-ps27-1.eml.xml` (Groningen) en `110a-ps27-2.eml.xml` (Limburg):
- PROVINCIE (nummer 1–12, zonder stembureau) > KIESKRING(en) met nummer en naam uit e-KS, niet hernummerd > GEMEENTE, per kieskring in de volgorde van e-KS.
- De kieskring waar het CSB zit krijgt CSB en HSB; de andere alleen HSB. Bij meer kieskringen heeft het HSB van de kieskring met het CSB `AcceptCentralSubmissions="true"`. Limburg: RomanNumerals op beide kieskringen.
- Noord-Holland volgens e-KS: 1 Amsterdam, 2 Den Helder, 3 Haarlem (CSB bij 3).
- kr:ElectionDomain bevat alleen de naam, zonder Id. ContestIdentifier "geen" bij één kieskring, "alle" bij meer.
- Naam: "Verkiezing van de provinciale staten van <provincie> <jaar>"; Id `PS<jaar>_<naam zonder accenten>`.
- NBSB (9010) wordt bij PS niet opgenomen.
- Getest (23-11-2026): Groningen (43 zetels) en Limburg (47 zetels) op 17-03-2027 zijn gelijk aan de e-KS-voorbeelden, inclusief volgorde en alle attributen (12 en 34 gebieden). Alle provincies samen: 341 gemeenten, geen zonder naam.

## Fries toegestaan (FrysianExportAllowed)
Volgens MasterElectionTree van e-KS staat het attribuut op het gebied van de verkiezing en op de gemeenten, nooit op een kieskring:
- **GR en BC/GC/bestuursraad:** op de gemeente als die een van de 18 Friese gemeenten is (`FRYS_GEM`). Westerkwartier (1969) hoort er niet bij: die ligt in Groningen.
- **PS Fryslân (2):** op de PROVINCIE en op alle 18 gemeenten; niet op kieskring 1 Leeuwarden.
- **AB Fryslân (2):** op het waterschap en zijn 19 gemeenten, dus ook Westerkwartier.
- **EK:** op PROVINCIE 2, zoals in 110a-ek27.
- **Niet** bij TK, EP, KC en fictieve gebieden, ook niet op Friese gemeenten in een TK- of EP-indeling (onduidelijk; e-KS heeft er geen voorbeeld van).
- Het vinkje "Fries" in de boom blijft met de hand aan of uit te zetten.

## Eerste Kamer (EK)
Opbouw zoals het e-KS-voorbeeld `110a-ek27.eml.xml`:
- Id `EK<jaar>`, naam "Verkiezing van de Eerste Kamer der Staten-Generaal <jaar>", geen ElectionDomain, ContestIdentifier "alle", 75 zetels, drempel 100, kandidaatstelling 34 dagen vóór de stemming.
- STAAT "Nederland" (zonder stembureau) > PROVINCIE 1–12 plus 13 Bonaire, 14 Sint Eustatius, 15 Saba en 16 Buitenland, elk met PROV_SB > STEMBUREAU met hetzelfde nummer en dezelfde naam (zonder stembureau). Volgorde: STAAT, PROVINCIE 1–16, STEMBUREAU 1–16 (33 gebieden).
- PROVINCIE 9 Zuid-Holland heeft daarnaast het CSB `CommitteeName="De Kiesraad" AcceptCentralSubmissions="true"`, vóór PROV_SB. Geen HSB; de HSB-optie is bij EK verborgen.
- Getest (23-11-2026): gelijk aan 110a-ek27; inlezen van 110a-ek27 geeft hetzelfde bestand.

## Kiescolleges (KC)
- Vier aparte kiescolleges, elk met een eigen CSB en definitie: KIESCOLLEGE 13 Bonaire, 14 Sint Eustatius, 15 Saba (KCCN) en 16 "Kiescollege niet-ingezetenen" (KCNI), nummers uit MasterElectionTree van e-KS.
- Zetels: KCCN = leden eilandsraad vanaf 2027 (Verhogingswet BES): Bonaire 11, Sint Eustatius 7, Saba 7. KCNI: 25 (aantal 2023), met waarschuwing; het hangt af van het aantal registraties (art. 5 Wet kiescollege niet-ingezetenen).
- Naam "Verkiezing van de leden van het kiescollege <Bonaire|Sint Eustatius|Saba|niet-ingezetenen> <jaar>"; Id `KCCN<jaar>_<eiland>` of `KCNI<jaar>`. Stemming 17-03-2027, kandidaatstelling 01-02-2027.
- e-KS heeft geen voorbeeld-110a voor een kiescollege: proef inlezen.

## Stadsdelen en gebieden (BC/GC)
- MasterElectionTree van e-KS kent DEELGEMEENTE onder de GEMEENTE, met zelf het CSB en HSB: Amsterdam 101 Centrum t/m 107 Nieuw-West (BC); Rotterdam 201 Centrum t/m 214 Rozenburg (GC). De keuzelijst koppelt "Stadsdeelcommissie …" en "Gebied …" aan dat nummer. Bestuurscommissie Weesp en de bestuursraden van 's-Hertogenbosch hebben geen nummer.
- Optie bij vraag 6, standaard uit: aan = GEMEENTE (zonder stembureau) > DEELGEMEENTE <nr> <e-KS-naam> met CSB en HSB; uit = één GEMEENTE met CSB en HSB. ElectionDomain blijft de gemeentecode.
- Inlezen herkent DEELGEMEENTE en zet de optie aan met het nummer.

## Waterschappen (AB)
Opbouw zoals het e-KS-voorbeeld `110a-ws27.eml.xml`:
- WATERSCHAP (nummer = ElectionDomain-Id) > KIESKRING 1 (naam waterschap) met CSB en HSB > GEMEENTE (RegionNumber zonder voorloopnullen), in de volgorde van e-KS.
- Naam: "Verkiezing van het algemeen bestuur van het waterschap <naam> <jaar>"; Id `AB<jaar>_<naam zonder accenten>`.
- **Zetels:** NumberOfSeats is alleen het aantal gekozen zetels (categorie ingezetenen). De 4 geborgde zetels (2 ongebouwd, 2 natuurterreinen) worden benoemd, niet gekozen, en horen er niet in (Waterschapswet art. 13; e-KS-voorbeeld Fryslân: 21, niet 25).

| Nr | Waterschap | Algemeen bestuur | Gekozen |
|---|---|---|---|
| 1 | Noorderzijlvest | 23 | 19 |
| 2 | Fryslân | 25 | 21 |
| 3 | Hunze en Aa's | 23 | 19 |
| 5 | Vechtstromen | 27 | 23 |
| 23 | Zuiderzeeland | 25 | 21 |
| 24 | Drents Overijsselse Delta | 29 | 25 |
| overige 15 | | 30 | 26 |

  Samen 518 gekozen zetels (gelijk aan 2023), 602 leden met de geborgde zetels. Alle waterschappen: AB2 en 25%.
- Meldingen: info bij het aantal uit het reglement; waarschuwing als het totaal inclusief geborgde zetels is ingevuld, of als het aantal anders afwijkt.
- Het fictieve waterschap Rivier en Polder heeft in de keuzelijst de waarde "F", omdat nummer 10 ook Amstel, Gooi en Vecht is.
- Getest (23-11-2026): Fryslân 2027 met 21 zetels is gelijk aan 110a-ws27. Alle waterschappen samen: 341 verschillende gemeenten, geen zonder naam.

## Bronnen
- Raadszetels en gemeentecodes: Kiesraad *Aantal raadzetels GR2026*, aangevuld met Hilversum (39 zetels, na samenvoeging met Wijdemeren per 1-1-2027).
- Statenzetels: Kiesraad-overzicht 2023. Kieskringen en gemeenten per kieskring (PS): MasterElectionTree.xml van e-KS. De TK/EP-indeling gebruikt nog *Kieskringen PS 2023*.
- Waterschapsnummers, gemeenten per waterschap, Fries toegestaan, EK-indeling, kiescollegenummers en deelgemeenten: e-KS (`MasterElectionTree.xml` en de voorbeelden in `src/models/eml/testdata/`, stand 23-11-2026).
- EK-drempel: parlement.com. Kiescollegeleden: Verhogingswet BES en Wet kiescollege niet-ingezetenen.
- Waterschapszetels: provinciale reglementen en websites van de waterschappen (gecontroleerd 23-11-2026); Delfland, Schieland en de Krimpenerwaard en Hollandse Delta uit het GS-ontwerpbesluit van Zuid-Holland, Noorderzijlvest uit het ontwerpbesluit en de website van het waterschap.
- RegionNumber van een gemeente is de CBS-code zonder voorloopnullen (0037 → 37). Een ElectionDomain-Id mag alleen 4 cijfers of 0 t/m 29 zijn.

## Open punten
- **XSD:** de app controleert de opbouw, maar valideert niet tegen de XSD van EML_NL. Altijd eerst proef inlezen.
- **Werk wordt niet bewaard:** de app gebruikt geen localStorage. Na herladen is alles weg; open dan het gedownloade 110a opnieuw.
- **Zetelgegevens verouderen:** raadszetels komen uit GR2026, statenzetels uit 2023, EP-zetels (31) uit 2024.
- **Landelijk register is een momentopname** (37 aanduidingen, stand 27-08-2026).
- **Waterschapszetels:** momentopname van de reglementen; voor vier waterschappen nog uit een ontwerpbesluit. Controleren bij de bekendmaking van de kandidaatstelling.
- **Kiescollege:** geen e-KS-voorbeeld; KCNI-leden (25) en eilandsraadleden (11/7/7) controleren bij de bekendmaking.
- **Eerste Kamer:** stemwaarden horen niet in dit bestand; de app doet daar niets mee.
- **BC/GC/bestuursraad:** niet bevestigd of de uitslagsoftware DEELGEMEENTE accepteert; daarom staat de optie standaard uit. Bestuursraad als categorie BC is een aanname.
- **HSB bij het CSB:** de app volgt standaard de Kiesraad-bestanden (HSB opgenomen), terwijl de Kieswet (E 10, E 11) die alleen bij TK, EP en PS met meerdere kieskringen kent. Nagaan wat Abacus verwacht.
- **Standaardindeling TK en EP:** is een uitgangspunt; extra controleren.
