# App: Uitslagen-generator (510b · 510c · 510d)

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
- Alles optioneel ondertekend, met een Abacus-importzip per bestand.

## 510b-formaat
Het formaat volgt `backend/src/eml/tests/eml510b_test.eml.xml` in kiesraad/abacus (EML_NL 1.3.1). Wat dat betekent:
- `kr:Schema`, `kr:CountingMethod` en `kr:Phase` staan in het bestand.
- Elk stembureau heeft een `kr:ReportingUnitType` en `kr:ReportingUnitInvestigations`.
- `Cast` = kiesgerechtigden (Abacus leest dit als `number_of_voters`).
- UncountedVotes: stempassen, volmachtbewijzen, kiezerspassen (niet bij GR), toegelaten kiezers, meer/minder getelde stembiljetten.

De afzender-ID's komen uit `backend/src/eml/committees.rs`: `CSB`, `HSB<n>` en de 4-cijferige gemeentecode. Bij HSB staat dat in Abacus nog uitgecommentarieerd (issue #3861).

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
