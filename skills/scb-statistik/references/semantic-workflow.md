# Semantisk tabellsökning: arbetsmetod

## Kandidater rangordnas med belägg

Sök brett med API:ets nyckelordssökning, men tolka frågan och jämför metadata själv. För varje kandidat, kontrollera:

| Dimension | Kontrollfråga |
|---|---|
| Mått | Mäter tabellen exakt folkmängd, eller exempelvis folkökning/flyttningar? |
| Population | Vilka personer eller hushåll omfattas? |
| Geografi | Finns rätt regionnivå och vald kommun/län/region? |
| Tid | Är det årsslut, månadsmedel, kvartal eller annan referenstid? Täcker serien önskade år? |
| Uppdelningar | Finns kön, ålder eller andra efterfrågade dimensioner? |
| Enhet | Är värdet personer, procent, index eller något annat? |
| Aktualitet | Jämför senaste referensperiod och uppdateringsdatum. Aktualitet väger högt först bland semantiskt jämförbara tabeller. |
| Jämförbarhet | Finns fotnoter om regionala indelningar, definitioner eller tidsseriebrott? |

Ranka först på träff i mått och population, sedan geografi och tidsbegrepp, sedan önskade uppdelningar. Använd aktualitet som utslagsgivare bland kandidater som faktiskt mäter samma sak. En nyare tabell om folkökning är inte bättre träff för en fråga om folkmängd.

## Exempel på frågetolkning

Fråga: “Hur har folkmängden i Nacka kommun utvecklats 2010–2024?”

- Mått: befolkningens storlek (`folkmängd`, bestånd), inte årlig förändring.
- Geografi: Nacka kommun; slå upp den aktuella regionkoden i just tabellens värdemängd.
- Tid: årsvärden. Bekräfta om tabellen avser 31 december eller annan årstidpunkt.
- Frekvens: årlig, om inte användaren ber om månadsdata.
- Enhet: normalt antal personer, verifieras i metadata.

Sök exempelvis med separata varianter: `folkmängd region år`, `befolkning kommun år`, `invånare Nacka`, `antal folkbokförda region`. Hämta aktuella tabellträffar, jämför metadata och välj först därefter tabell. Portalens visningstitel eller äldre kod är bara en ledtråd; aktuell API-metadata avgör.

## Validera uttaget

Innan datahämtning: kontrollera att varje vald dimension har en giltig kod; kontrollera att en single-select dimension inte får flera värden; inkludera bara begärda kategorier; beräkna ungefärligt antal celler; och kontrollera responsens tidsstämplar/enhet innan resultatet sammanfattas. Redovisa bortfall eller avvikande år tydligt.
