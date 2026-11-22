# Wijzigingen

Wat er per update is veranderd, nieuwste bovenaan. De precieze verschillen per bestand staan in de [commitgeschiedenis](https://github.com/Bonzo261/EML-hulpmiddelen/commits/main).

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
