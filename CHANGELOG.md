# Wijzigingen

Wat er per update is veranderd, nieuwste bovenaan. De precieze verschillen per bestand staan in de [commitgeschiedenis](https://github.com/Bonzo261/EML-hulpmiddelen/commits/main).

## 25-11-2026: map `huisstijl/`

- Het logo, de iconen en de lettertypen voor de PDF's van de PV-generator staan nu samen in de map `huisstijl/` in plaats van los in de hoofdmap. Alle pagina's verwijzen naar de nieuwe plek. Voor gebruikers verandert er niets.
- Wie de site lokaal gebruikt: laat de map `huisstijl/` naast de HTML-bestanden staan.

## 25-11-2026: inleveringsbestanden (210)

### Kandidatenlijsten-maker (`kandidatenlijsten.html`)
- **Nieuw: EML 210**, het bestand waarmee een partij een kandidatenlijst inlevert, zoals [e-KS](https://github.com/kiesraad/e-ks) dat gebruikt. Bedoeld om het inleveren in e-KS te testen met losse kandidatenlijsten.
  - Eén bestand per ingeleverde lijst, in de map `210_inlevering/` van de zip. Een partij met overal dezelfde lijst levert één bestand in; bij regionale stellen of een lijstengroep zijn het er meer.
  - Per kandidaat staat er een (verzonnen) adres en een fictief BSN in. De BSN's halen de elfproef en komen uit een reeks die niet aan echte personen wordt uitgegeven. Elke persoon houdt in alle bestanden hetzelfde BSN.
  - De inleveraar en een of twee plaatsvervangers staan erin, met naam en adres.
- Bij vraag 4 zet je de 210 aan (standaard uit). Je kunt kiezen of het BSN erin komt en hoeveel plaatsvervangers er zijn.
- In de weergave kun je elk 210-bestand bekijken.
- De 230b- en 230c-bestanden zijn niet veranderd. Met hetzelfde startgetal krijg je dezelfde personen.
- De opbouw is vergeleken met de e-KS-voorbeelden voor waterschap Fryslân, Provinciale Staten Groningen en Limburg en de Eerste Kamer.

### Handleidingen
- Handleiding van de Kandidatenlijsten-maker bijgewerkt voor de 210.

### Website
- De kaart van de Kandidatenlijsten-maker op de startpagina noemt nu ook 210.
- `docs/kandidatenlijsten-maker.md`, de README en `WENSEN.md` bijgewerkt.

## 23-11-2026 (avond): totaallijsten met adressen (230c)

### Kandidatenlijsten-maker (`kandidatenlijsten.html`)
- **Nieuw: EML 230c**, de totaallijsten met adressen, zoals [e-KS](https://github.com/kiesraad/e-ks) die maakt. Alle kieskringen staan in één bestand, `Totaallijsten_<verkiezing>.eml.xml`. Dit is het totaalbestand voor het centraal stembureau.
  - Een kandidaat die in Nederland woont, krijgt een (verzonnen) correspondentieadres met postcode.
  - Een kandidaat in het buitenland krijgt een gemachtigde.
  - Elke persoon krijgt een korte code. Staat dezelfde persoon in een volgende kieskring op de lijst, dan verwijst het bestand alleen naar die code.
- Bij vraag 4 kies je welke bestanden je wilt: de 230c (standaard aan), een 230b per kieskring (standaard aan) en eventueel nog de oude 230b met alle kieskringen. De 230b per kieskring blijft nodig voor Abacus, want Abacus leest geen 230c.
- Bij één kieskring kun je in de weergave kiezen tussen de 230b en de 230c.
- De 230b-bestanden zelf zijn niet veranderd. Met hetzelfde startgetal krijg je dezelfde personen als eerst.
- De opbouw is vergeleken met de e-KS-voorbeelden voor waterschap Fryslân, Provinciale Staten Limburg en de Eerste Kamer.

### Handleidingen
- Handleiding van de Kandidatenlijsten-maker bijgewerkt, met een nieuwe paragraaf "De 230c".

### Website
- De kaart van de Kandidatenlijsten-maker op de startpagina noemt nu ook 230c.
- `docs/kandidatenlijsten-maker.md`, de README en `WENSEN.md` bijgewerkt.

## 23-11-2026 (ochtend, 2): logo en nieuwe startpagina

- **Logo:** de site heeft een eigen logo: twee haken met een rood potlood. Het logo staat in de kop van de startpagina en is ook het icoon in het browsertabblad en op het beginscherm van je telefoon, op alle pagina's.
- **Nieuwe startpagina:** een korte uitleg voor wie de apps gebruikt, een duidelijkere disclaimer ("Niet verbonden aan de Kiesraad"), genummerde app-kaarten met het EML-bestand dat ze maken of lezen, en nieuwe teksten bij de Verkiezingsdefinitie-maker en de Kandidatenlijsten-maker.
- De voettekst noemt de broncode op GitHub, de licentie en de bronnen (Abacus en e-KS).

## 23-11-2026 (ochtend): aansluiten op e-KS

De Verkiezingsdefinitie-maker en de Kandidatenlijsten-maker maken hun bestanden nu zoals [e-KS](https://github.com/kiesraad/e-ks), het nieuwe kandidaatstellingssysteem van de Kiesraad. De indeling van Nederland komt uit de `MasterElectionTree.xml` van e-KS. De bestanden zijn vergeleken met de e-KS-voorbeelden voor Provinciale Staten Groningen en Limburg, waterschap Fryslân en de Eerste Kamer, en zijn daaraan gelijk.

### Verkiezingsdefinitie-maker (`verkiezingsdefinitie.html`)
- **Waterschappen:** de 21 echte waterschappen staan in de keuzelijst, met hun nummer en gemeenten. Het aantal zetels wordt automatisch ingevuld: alleen de gekozen zetels, zonder de 4 geborgde zetels (bijvoorbeeld Fryslân 21 in plaats van 25). Een waarschuwing volgt als je het totaal inclusief geborgde zetels invult.
- **Provinciale Staten:** kieskringen en gemeenten volgens e-KS, met de nummering van e-KS (Noord-Holland nu 1 Amsterdam, 2 Den Helder, 3 Haarlem). De naam wordt "Verkiezing van de provinciale staten van …".
- **Eerste Kamer:** de indeling is nu gelijk aan het e-KS-voorbeeld (12 provincies, de 3 eilanden en het buitenland, met de Kiesraad als centraal stembureau). Drie fouten opgelost: de voorkeurdrempel is 100% (niet 50%), de kandidaatstelling is op de 34e dag vóór de stemming (niet de 44e), en de kandidaatstelling geldt voor "alle" kieskringen.
- **Kiescolleges:** nu vier aparte kiescolleges (Bonaire, Sint Eustatius, Saba en niet-ingezetenen) in plaats van één, met het aantal leden vanaf 2027.
- **Stadsdelen en gebieden:** de stadsdelen van Amsterdam en de gebieden van Rotterdam kunnen als DEELGEMEENTE in de indeling, zoals e-KS. Deze optie staat standaard uit.
- **Fries toegestaan** staat nu ook bij Provinciale Staten Fryslân en bij de 18 Friese gemeenten.

### Kandidatenlijsten-maker (`kandidatenlijsten.html`)
- Kandidaten krijgen een **geboortedatum** (18 t/m 80 jaar op de dag van stemming). Deze optie staat standaard aan.
- **Geslacht onbekend** is mogelijk, met een instelbaar percentage.
- In `kr:ListData` kun je instellen of het geslacht gepubliceerd wordt en in welke taal de lijst gepubliceerd wordt: Nederlands, of Fries als de verkiezing dat toestaat.
- Kandidaten in het buitenland kunnen nu bij alle soorten verkiezingen.
- Bij Limburg is de kieskring nu "1" zoals in e-KS. Met een schakelaar kies je Romeinse cijfers ("I"), zoals Abacus verwacht.
- Met hetzelfde startgetal krijg je nog steeds dezelfde namen en woonplaatsen als in eerdere versies.
- De woonplaatsen bij Provinciale Staten in Gelderland, Noord-Holland en Noord-Brabant passen weer bij de juiste kieskring.

### Handleidingen
- De handleidingen van de Verkiezingsdefinitie-maker en de Kandidatenlijsten-maker zijn herschreven, met nieuwe paragrafen over Provinciale Staten, waterschappen, de Eerste Kamer, kiescolleges, stadsdelen en Fries.

### Website
- Nieuw: [`WENSEN.md`](WENSEN.md), met wat er nog op de to-do-lijst staat.
- `docs/verkiezingsdefinitie-maker.md`, `docs/kandidatenlijsten-maker.md` en de README bijgewerkt.

## 23-11-2026: controle zetelverdeling rekent volgens de wettekst

### Controle zetelverdeling (`zetelverdeling.html`)
- Nieuwe hoofdberekening, rechtstreeks uit de tekst van Kieswet P 5 t/m P 10, met bij elke stap de zin uit de wet. Zo is de controle onafhankelijk van de uitslagsoftware. Eerst rekende de app met een overgenomen versie van de Abacus-berekening, en daarmee zouden fouten van Abacus ook in de controle terechtkomen.
- De Abacus-berekening blijft als toetsing. Nieuw onderdeel 6 "Toetsing van deze berekening" in stap 2 meldt of beide op dezelfde zetels uitkomen, met de verschillen per lijst.
- Punten waar de wet ruimte voor uitleg laat, worden gemeld: doen lijsten onder 75% mee bij P 8 lid 3 (in te stellen), wat als er daarna nog zetels over zijn, en bijzondere gevallen bij P 9.
- P 10 werkt nu als "voortgezette toepassing": een lijst zonder kandidaten meer doet niet meer mee.
- Bij een loting voor P 9 kies je de lijst waarvan de zetel vervalt. Nieuwe knop "Vorige loting ongedaan maken".
- Nieuw voorbeeld "Loting en lijstuitputting": hier vraagt Abacus om een loting die volgens de wettekst niet nodig is. De zetels komen in beide gevallen hetzelfde uit.
- Het verslag noemt ook de uitlegkeuzes en de uitkomst van de toetsing.

### Handleidingen
- Handleiding van de Controle zetelverdeling bijgewerkt voor de nieuwe werking, met uitleg over de toetsing en de uitlegpunten.

### Website
- `docs/zetelverdeling-controle.md` en de README bijgewerkt.

## 22-11-2026 (nacht): link naar de website
- De site staat online op <https://bonzo261.github.io/EML-hulpmiddelen/>. De README verwijst daar nu naar, in plaats van uit te leggen hoe GitHub Pages aangezet moet worden.

## 22-11-2026 (laat): controle zetelverdeling en controleprotocol GR26

### Nieuw: Controle zetelverdeling (`zetelverdeling.html`)
- Het CSB rekent de zetelverdeling zelf na volgens Kieswet P 5 t/m P 10: kiesdeler, volle zetels, restzetels (grootste gemiddelden bij 19 of meer zetels, grootste overschotten met 75%-drempel daaronder), P 9 (volstrekte meerderheid) en P 10 (te weinig kandidaten).
- Rekent met exacte breuken. De rekenkern is overgenomen uit de zetelverdeling van Abacus en is getest met alle 46 eind-tot-eindtests daarvan.
- Extra ten opzichte van Abacus: de kiesdelerdrempel voor restzetels bij de Tweede Kamer en het Europees Parlement.
- Loting bij gelijke stand: de app vraagt de uitkomst van de loting van het CSB en bewaart die.
- Invoer uit 510b/c/d, 110a, 230b en 520, of plakken uit Excel (ook uit de spreadsheet van de Kiesraad).
- Vergelijking met de uitkomst van de uitslagsoftware (520 of overtikken van het PV) en een verslag voor onderdeel B van het controleprotocol CSB.

### Controleprotocol (`controleprotocol.html`): herschreven voor GR26
- Volgt nu alleen de controleprotocollen GR26 van de Kiesraad (november 2025).
- GSB: keuze voor stemopneming DSO of CSO. Schermen, Excel en verslag noemen daarna de juiste processen-verbaal en rubrieken.
- CSB, en HSB met één gemeente: altijd alle lijsten, zonder loting. Het CSB werkt alleen met hoofdstembureaus als er meer dan één kieskring is.
- De loting krijgt een controlecode.
- Nieuw: offline invoer in Excel. De app maakt een beveiligd invulbestand met de gelote lijsten en leest het ingevulde bestand weer in, met controle op de code, de koppen en de getallen. Hiervoor is geen externe bibliotheek meer nodig.
- Kenmerken van de uitslag: de app rekent de hashcode uit en controleert de eerste 8 tekens tegen de voettekst van het PV. Elke ingelezen uitslag komt in een uitslaggeschiedenis.
- Bij een verschil toont de app de stappen 5 en 6 van het GR26-protocol. De knop "Alle lijsten controleren" is weggehaald.
- Verslag per niveau: voor het GSB een tekst voor rubriek 2.5 van het PV, voor het CSB onderdeel B (lijsttotalen).

### Handleidingen (`handleidingen.html`)
- Handleiding van het Controleprotocol herschreven voor GR26, met uitleg over de offline invoer in Excel.
- Nieuwe handleiding voor de Controle zetelverdeling.

### Website
- Startpagina: zeven apps in drie groepen (basisbestanden maken, uitslagen testen, uitslag controleren).
- Nieuwe notitie `docs/zetelverdeling-controle.md`; `docs/controleprotocol.md`, `docs/onderhoud.md` en de README bijgewerkt.
- Dit wijzigingenlogboek toegevoegd.

## 22-11-2026 (avond): PV-generator betrouwbaarder

### PV-generator (`pv-generator.html`)
- De PDF bleef op de echte site soms eindeloos draaien. De app haalt de onderdelen voor de PDF (pdfmake en JSZip) nu bij jsDelivr op, met unpkg en cdnjs als reserve.
- Lukt het laden niet of duurt het opmaken langer dan 90 seconden, dan stopt de app met een foutmelding.

### Handleidingen
- Uitleg dat de PV-generator internet nodig heeft, en twee nieuwe vragen over deze foutmeldingen.

## 22-11-2026: eerste versie
- Zes apps: Verkiezingsdefinitie-maker (110a), Stembureaus-maker (110b), Kandidatenlijsten-maker (230b), Uitslagen-generator (510b/c/d), PV-generator en Controleprotocol.
- Handleidingen, startpagina, README en technische notities in `docs/`.
- Licentie EUPL-1.2, dezelfde als Abacus.
