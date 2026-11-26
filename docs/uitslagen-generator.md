# App: Uitslagen-generator (510b · 510c · 510d · 520)

Bestand: `uitslagen-generator.html`

**Doel:** test-uitslagen genereren om Abacus te testen, ook de HSB- en CSB-functionaliteit die nog in ontwikkeling is.

**Invoer:**
- 110a (verkiezingsdefinitie)
- één of meer 230b (kandidatenlijsten, per kieskring)
- één of meer 110b (stembureaus, per gemeente)

**Uitvoer:**
- 510b per gemeente (GSB)
- 510c per HSB/kieskring
- 510d (CSB)
- 520 (CSB): zetelverdeling en gekozen kandidaten (sinds 26-11-2026)
- Alles optioneel ondertekend, met een Abacus-importzip per bestand.

**Getest tegen het XSD:** alle soorten (510b/c/d en 520) zijn geldig tegen het officiële XSD uit github.com/kiesraad/EML_NL. Getest met het voorbeeld en met de testbestanden van Abacus (GR, TK, PS2).

## 510b-formaat
Het formaat volgt `backend/src/eml/tests/eml510b_test.eml.xml` in kiesraad/abacus (EML_NL 1.3.1). Wat dat betekent:
- `kr:Schema`, `kr:CountingMethod` en `kr:Phase` staan in het bestand.
- Elk stembureau heeft een `kr:ReportingUnitType` en `kr:ReportingUnitInvestigations`.
- `Cast` = kiesgerechtigden (Abacus leest dit als `number_of_voters`).
- UncountedVotes: stempassen, volmachtbewijzen, kiezerspassen (niet bij GR), toegelaten kiezers, meer/minder getelde stembiljetten.

De afzender-ID's komen uit `backend/src/eml/committees.rs`: `CSB`, `HSB<n>` en de 4-cijferige gemeentecode. Bij HSB staat dat in Abacus nog uitgecommentarieerd (issue #3861).

## 520-formaat (XSD `520-result-kiesraad-strict.xsd` en de documentatie in EML_NL)
- Alleen gekozen lijsten en kandidaten, met `Elected` = yes (vast in het XSD).
- Per lijst met zetels eerst een Selection met AffiliationIdentifier, daarna de gekozen kandidaten.
- **`CandidateIdentifier Id` is de volgorde van verkiezing binnen de lijst (1 = eerste zetel), niet het nummer op de lijst.** ShortCode wordt meegenomen als de 230b die heeft.
- CandidateFullName (xnl:PersonName), Gender en QualifyingAddress worden uit de 230b overgenomen. QualifyingAddress is verplicht; ontbreekt die, dan vult de app de plaatsnaam van de verkiezing in.
- `Votes` = voorkeurstemmen (alleen als > 0, want het XSD vraagt een positiveInteger).
- `Ranking`: 1 = gekozen met voorkeurstemmen, 2 = op lijstvolgorde.
- De afzender is dezelfde als bij de 510d (standaard `CSB`). Ondertekend met de CSB-sleutel.

**Rekenregels voor de 520:**
- Zetels uit 110a `kr:NumberOfSeats`, of uit het veld in de app.
- Zetelverdeling met de wetskern `ZVW.verdeel` uit de app Controle zetelverdeling (Kieswet P 5–P 10). Kiesdelerdrempel bij TK en EP.
- Voorkeurdrempel uit 110a `kr:PreferenceThreshold`. Anders 50% bij GR met minder dan 19 zetels en 25% in alle andere gevallen. Zo doet Abacus het ook (`get_eml_preference_threshold`).
- P 15: kandidaten met meer stemmen dan de drempel (strikt groter, zoals in Abacus) worden gekozen in volgorde van stemmen; bij gelijke stemmen telt de lijstvolgorde. Zijn er meer van zulke kandidaten dan zetels, dan beslist het lot.
- P 17: de overige zetels gaan in lijstvolgorde.
- Lotingen (restzetel, P 9, kandidaten) trekt de app met de seed. Ze worden gemeld in de controlelijst.

**Beperking:** bij meerdere kieskringen (TK, PS met meer kieskringen) worden kandidaten per lijst samengenomen op kandidaatnummer en de volgorde van de eerste kieskring. P 17–P 19 per kieskring wordt niet nagebootst. Overleden kandidaten worden niet ondersteund.

## Ondertekening (Abacus `backend/eml_signature`, compatibel met OSV2020-U)
**Sleutel en certificaat:**
- Per orgaan (GSB, HSB, CSB) een RSA-4096-sleutel.
- Zelfondertekend X.509v3-certificaat met sha256WithRSA en een serienummer van 160 bit, zonder extensies.
- Subject: C=NL, L, O=<ElectionIdentifier Id>, OU, CN, UID.
- Geldig tot 3 maanden na de verkiezing.

**UID per orgaan (instelbaar in de app):**
- GSB: `PSB<code>`. Dit is officieel en wordt door Abacus nu ondersteund.
- HSB: `HSB<n>`. Aanname: zo staat het in de Abacus-test `rejects_unsupported_uid_prefix`, en het komt overeen met de afzender-ID.
- CSB: `CSB`. Aanname.
- CN is "Gemeente X", "Hoofdstembureau X" of "Centraal stembureau X". Dit veld is alleen voor weergave.

**Handtekening (`.signature`):**
- CMS signedData in DER, versie 1, digest SHA-256.
- id-data met een lege OCTET STRING, plus het certificaat.
- SignerInfo zonder signed attributes, met rsaEncryption over de exacte bytes van het EML-bestand.

**Import in Abacus:**
- Een zip met precies één `X.eml.xml` en `X.eml.xml.signature`.
- Abacus controleert de handtekening tegen een eerder ingelezen `.crt`.
- Daarna vraagt Abacus om een deel van de SHA-256, in 16 groepen van 4 hex-tekens.

**Getest:** OpenSSL keurt alle handtekeningen (GSB, HSB, CSB) goed. De ASN.1-structuur is gelijk aan de fixture van Abacus. Nog niet getest in een draaiende Abacus.

**Sleutels:**
- Bewaard in localStorage, per verkiezing + UID.
- Export en import als .json.
- Alle .crt-bestanden zijn als zip te downloaden.

## Open punten
- Het certificaatformaat voor HSB en CSB nalopen zodra Abacus het vastlegt.
- ReportingUnit-ID's voor 510c/510d (`HSB{n}::{gemeente}`, `HSB{n}`) en CSB-contest `alle` zijn nog niet tegen een echt bestand gecontroleerd.
- 510d telt kandidaatstemmen op per lijst-Id + kandidaat-Id.
- 510d en 520 tellen kandidaatstemmen op per lijst-Id + kandidaat-Id.
- De 520 testen tegen een echte 520 van OSV of de Kiesraad, en met de app Controle zetelverdeling.
