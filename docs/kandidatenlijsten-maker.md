# App: Kandidatenlijsten-maker (230b)

Bestand: `kandidatenlijsten.html`

**Doel:** kandidatenlijsten (EML 230b) met fictieve personen maken voor test en oefening, per kieskring, met hashcodes.

**Invoer:** een 110a uit de Verkiezingsdefinitie-maker of de uitslagsoftware. Voorbeelden: gemeenteraad en Tweede Kamer.

**Uitvoer:** een zip met de 230b-bestanden, per bestand een hashcode-txt en een `Hashcodes_overzicht.txt`. Altijd een totaalbestand met alle kieskringen (nodig voor de uitslagvaststelling door het CSB), optioneel ook één bestand per kieskring (voor de HSB's).

## Vragen
1. **Verkiezingsdefinitie inladen** (110a). De rest is vergrendeld tot dit gebeurd is.
2. **Aantal partijen.** De eerste krijgen de geregistreerde aanduidingen uit de 110a, de rest een fictieve naam. Per lijst aan te passen: aanduiding (leeg = blanco lijst), aantal kandidaten, en per kieskring:
   - **gelijk**: overal dezelfde lijst (stel gelijkluidende lijsten);
   - **regionale stellen**: zelfde top en lijstduwer, regionale kandidaten per groep kieskringen;
   - **elke kieskring anders**: lijstengroep;
   - **alleen enkele kieskringen**: de partij doet niet overal mee (bijv. `1,3-5`).
3. **Personen:** aandeel vrouwen (%), startgetal (zelfde startgetal = zelfde personen), knop `Nieuwe personen`, optioneel enkele kandidaten in het buitenland (alleen TK en EP).
4. **Bestanden:** apart bestand per kieskring, `kr:ListData` opnemen (EML_NL 1.3+). Geavanceerd: ManagingAuthority Id en naam, TransactionId, IssueDate.

## Resultaat rechts
- Kieskringen, lijsten, unieke kandidaten, aantal bestanden.
- Weergave als stembiljet (per kieskring; rood nummer = regionale kandidaat), een totaaloverzicht van alle kieskringen (S = stel, G = lijstengroep, Z = op zichzelf staand), of als XML.

## Regels
- Maximaal 50 kandidaten per lijst, 80 voor partijen met meer dan 15 zetels.
- ManagingAuthority Id alleen CSB, HSB+nummer, SB+nummer of een 4-cijferige gemeentecode.
- Hashcode is SHA-256 over het hele bestand, in groepen van vier zoals op de PV's.
- Alle personen zijn fictief; alleen voor test en oefening.

## Open punten
- **XSD:** de app valideert niet tegen de XSD. Altijd eerst proef inlezen.
- **Abacus leest per bestand één kieskring.** Het totaalbestand met alle kieskringen is daar niet voor bedoeld; gebruik in Abacus de bestanden per kieskring.
- **Lijstnummers zijn in alle kieskringen gelijk.** Een afwijkende nummering per kieskring kan niet.
- **Lijsten met minder dan 4 kandidaten** worden altijd als ‘gelijk’ gemaakt, ook als regionale stellen of lijstengroep is gekozen. De app meldt dit niet.
- **Instellingen worden niet bewaard.** Bij elke start krijgt het startgetal een willekeurige waarde; noteer het startgetal om dezelfde personen terug te krijgen.
- **Woonplaatsen** komen uit de gemeenten in de 110a, of uit een vaste lijst per TK-kieskring. Ze zijn indicatief.
- **Grenzen:** maximaal 60 partijen; de XML-weergave toont maximaal 2500 regels (de download is volledig).
