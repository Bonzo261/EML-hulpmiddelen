# App: Verkiezingsdefinitie-maker (110a)

Bestand: `verkiezingsdefinitie.html`

**Doel:** een verkiezingsdefinitie (EML 110a) maken die je in de uitslagsoftware kunt inlezen. Het bestand is de basis voor alle andere apps (stembureaus, kandidatenlijsten, PV's, uitslagen).

**Invoer:** de vragen op het scherm, of een bestaand 110a-bestand (`Bestaand 110a-bestand openen`).

**Uitvoer:** `.eml.xml` met een SHA-256-hashcode, en de XML om te kopiëren.

## Vragen
1. **Soort verkiezing:** GR, PS, AB (waterschap), TK, EP, EK, Kiescollege (vanaf EML_NL 1.3), stadsdeelcommissie (BC), wijkraad/gebiedscommissie (GC) en bestuursraad (BC; EML_NL kent geen aparte categorie).
2. **Gebied:** gemeente, provincie of waterschap uit een keuzelijst, of zelf invullen. Gemeentecode = 4-cijferige CBS-code met voorloopnullen. Provincienummers 1 (Groningen) t/m 12 (Limburg). Bij BC/GC/BR ook de naam van de commissie.
3. **Datums:** dag van stemming; dag van kandidaatstelling automatisch de 44e dag ervoor (aan te passen).
4. **Zetels:** aantal zetels, voorkeurdrempel en subcategorie worden automatisch bepaald. Voorkeurdrempel: 25% van de kiesdeler, 50% bij minder dan 19 zetels en voor de EK, 10% voor het EP. PS met één kieskring = PS1, meer kieskringen = PS2.
5. **Geregistreerde aanduidingen:** letterlijk uit het register van het CSB, één per regel. Knop voor het landelijk register (37 aanduidingen, stand 27-08-2026). Inlezen uit 110a, 230b, txt of csv; aanvullen of vervangen.
6. **Indeling (verkiezingsboom):** gebieden met categorie, nummer, naam, bovenliggend gebied, stembureaus (CSB/HSB/PROV_SB). Opties:
   - HSB opnemen bij het CSB zoals de Kiesraad-bestanden doen (de Kieswet kent alleen een HSB bij TK, EP en PS met meerdere kieskringen);
   - fictieve testindeling (Heemdamseburg, Juinen, Middelgein, 's-Gravenveen, Oost-Holland, Zuid-Brabant, Rivier en Polder, uit de Abacus-testbestanden);
   - gemeenten opnemen in de indeling;
   - gemeenten in één keer plakken als `code;naam;kieskringnummer`.
7. **Bestand:** EML_NL-versie 1.3.1 (huidig), 1.3 of 1.2.2 (herindelingsverkiezingen 18-11-2026); naam en Id automatisch. Geavanceerd: TransactionId, IssueDate, ManagingAuthority Id, MaxVotes, ContestIdentifier.

## Bronnen
- Raadszetels en gemeentecodes: Kiesraad *Aantal raadzetels GR2026*, aangevuld met Hilversum (39 zetels, na samenvoeging met Wijdemeren per 1-1-2027).
- Statenzetels: Kiesraad-overzicht 2023. Kieskringen en HSB's: *Kieskringen PS 2023*.
- RegionNumber van een gemeente is de CBS-code zonder voorloopnullen (0037 → 37). Een ElectionDomain-Id mag alleen 4 cijfers of 0 t/m 29 zijn.

## Open punten
- **XSD:** de app controleert de opbouw, maar valideert niet tegen de XSD van EML_NL. Altijd eerst proef inlezen.
- **Werk wordt niet bewaard:** de app gebruikt geen localStorage. Na herladen is alles weg; open dan het gedownloade 110a opnieuw.
- **Zetelgegevens verouderen:** raadszetels komen uit GR2026, statenzetels en kieskringen uit 2023, EP-zetels (31) uit 2024. Bijwerken voor latere verkiezingen.
- **Landelijk register is een momentopname** (37 aanduidingen, stand 27-08-2026). Moet met de hand worden bijgewerkt.
- **Waterschappen:** de keuzelijst bevat alleen het fictieve waterschap Rivier en Polder. Echte waterschappen (nummer, zetels, indeling) moeten met de hand.
- **Kiescollege:** aantal leden en indeling worden niet automatisch ingevuld.
- **Eerste Kamer:** stemwaarden horen niet in dit bestand; de app doet daar niets mee.
- **BC/GC/bestuursraad:** EML_NL kent geen gebiedscategorie voor stadsdelen of wijken. De indeling gebruikt de gemeente; de naam van de raad staat in ElectionDomain en ElectionName. Bestuursraad als categorie BC is een aanname. Proef inlezen.
- **HSB bij het CSB:** de app volgt standaard de Kiesraad-bestanden (HSB opgenomen), terwijl de Kieswet (E 10, E 11) die alleen bij TK, EP en PS met meerdere kieskringen kent. Nagaan wat Abacus verwacht.
- **Standaardindeling TK, EP, EK:** is een uitgangspunt; extra controleren.
