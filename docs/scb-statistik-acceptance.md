# Statistical acceptance cases for scb-statistik

Use these cases alongside the existing offline document checks, not as a replacement for them. The reference values below were retrieved from SCB PxWebApi v2 on **2026-10-22**. They are dated published values, including disclosure protection where applicable; revisions must be reviewed rather than treated automatically as regressions.

## Run and assess

Run the question in a fresh conversation with the skill under review and access to SCB. Give the answering assistant the question and the skill, not the reference answer. Record the skill commit, model, date, API selections, source responses and final answer. Assess the answer only after execution against the reference below and current metadata. Repeat important cases with different wording before claiming stable behavior.

Require the correct statistical unit/population, measure, geography/direction, reference period, units and interpretation. A correct number with a materially wrong meaning fails. Mark blocked source access as unverified, not passed. Separate structural checks, live API behavior and model-answer quality in the result.

## Household questions

### H1 — Household count, population and coverage

**Question:** Hur många hushåll fanns i Malmö vid utgången av 2025? Omfattar hushållsuppgifterna alla Malmöbor, och hur stora är hushållen i genomsnitt?

**Reference:** `TAB4374`, `Region=1280`, `Tid=2025`; `ContentsCode=000000M4` 174 327 households; `000000MH` 367 924 residents; `000000MG` 366 317 residents with household information; `000000M3` published coverage 99.6 percent; `000000M5` published mean 2.10 persons per household. Reference date: 31 December. Explain incomplete coverage. Use the published mean based on people with household information, not all residents divided by households. Treat the 2025 values as CKM-protected.

### H2 — Household types and non-additivity

**Question:** Redovisa Malmös hushållstyper 2025 och kontrollera att de summerar till totalen.

**Reference:** `TAB6568`, `Region=1280`, `Tid=2025`, `ContentsCode=000007Y1`:

| Type code | Published households |
| --- | ---: |
| SBMB | 35 340 |
| SBUB | 34 402 |
| ESMB | 13 829 |
| ESUB | 77 363 |
| OVRIGA | 13 392 |
| TOTALT | 174 327 |

The five components sum to 174 326. Retain the directly published total and explain that non-additivity is consistent with CKM. Do not invent a with-/without-children subdivision of OVRIGA or call this discrepancy proof of an API error.

### H3 — Children and geographic comparability

**Questions:** Kan hushåll med barn i TAB6568 tolkas som hushåll med barn under 18 år? Kan ett DeSO jämföras direkt mellan 2023 och 2025?

**Reference:** The table's child definition has no general age limit; an under-18 question requires an appropriate age breakdown in another table. DeSO 2025/RegSO 2025 applies from reference year 2024; check boundaries and correspondence before comparing individual areas. No numeric under-18 answer or area-specific time series has been validated by this case. Earlier years in TAB6568 also have CTA disclosure treatment.

## Commuting questions

### C1 — In-/out-commuting and direction

**Question:** Hur många pendlade in och ut ur Malmö, Lund och Helsingborg 2024, och vad var pendlingsnettot?

**Reference:** `TAB1828`, `Kon=1+2`, `Tid=2024`; `000000QI` in, `000000QJ` out, `000000QK` resident/workplace in the same municipality:

| Municipality | In | Out | Same municipality | Calculated in minus out |
| --- | ---: | ---: | ---: | ---: |
| Malmö (1280) | 82 903 | 51 969 | 127 748 | +30 934 |
| Lund (1281) | 43 571 | 24 986 | 35 807 | +18 585 |
| Helsingborg (1283) | 28 426 | 22 970 | 49 485 | +5 456 |

Report employed people aged 15–74, not trips. Label the net as calculated from published values. Do not require exact additivity under disclosure protection.

### C2 — Workplace direction versus actual journeys

**Questions:** Hur många bor i Malmö och arbetar i Lund, och hur många bor i Lund och arbetar i Malmö 2024? Hur många arbetsresor per dag motsvarar detta?

**Reference:** `TAB1830`, `Kon=1+2`, `ContentsCode=000000QT`, `Tid=2024`; residence 1280/workplace 1281 gives 13 493; residence 1281/workplace 1280 gives 12 898. Daily journeys cannot be determined from these employed-person counts. Do not multiply by two or by working days without explicitly requested and supported modelling assumptions.

### C3 — Denominator and method break

**Questions:** Hur stor är utpendlingsandelen? Kan förändringen 2023–2024 tolkas som en ren förändring i pendlingen?

**Reference:** Require employed resident population with the same year, ages and coverage; check treatment of people working abroad. The entire municipal population and workplace population are wrong denominators. An out/(out+same municipality) calculation is conditional on those categories covering the intended population; the complete denominator and cross-border coverage are not certified by this case. Flag the 2024 change in classification of entrepreneurs and assignment of people without a linked workplace to their residence municipality. Do not attribute a measured change entirely to real travel behavior.

## Migration questions

### M1 — Table transition, direction and international shares

**Question:** Hur förändrades Malmös in- och utflyttningar mellan 2024 och 2025? Hur stor andel kom från respektive gick till utlandet?

**Reference:** `Region=1280` in both tables:

| Measure | TAB1212 code / 2024 value | TAB6640 code / 2025 value |
| --- | --- | --- |
| Inflyttningar | BE0101AU / 23 981 | 0000086B / 22 434 |
| Utflyttningar | BE0101AV / 22 327 | 0000086F / 22 101 |
| Invandringar | BE0101AX / 6 956 | 00000869 / 5 866 |
| Utvandringar | BE0101AY / 6 281 | 00000867 / 5 627 |
| Flyttningsöverskott | BE0101AZ / +1 654 | 00000868 / +333 |

For the older table, `Alder=tot` with `Kon` omitted returned an all-sex aggregate (verify the response). For the newer table, the explicit total selection was `Alder=TOT1`, `Kon=TotSa`. Resolve live codes rather than copying these indefinitely. Avoid adding overlapping age classifications or totals.

Calculated incoming international share: approximately 29.0 percent in 2024 and 26.1 percent in 2025; outgoing international share: 28.1 and 25.5 percent. Use all in-/out-movements as denominators; these already include international movements. Label derived shares and the introduction of CKM in 2025. Do not claim they are separately published exact rates.

### M2 — Protected totals

**Question:** Stämmer summan av inrikes och utrikes flyttningar med Malmös totaler 2025?

**Reference:** `TAB6640` with the same explicit totals as M1: inrikes in (`0000086D`) 16 571 plus immigration 5 866 gives 22 437 versus published in 22 434. Inrikes out (`0000086E`) 16 470 plus emigration 5 627 gives 22 097 versus published out 22 101. Inrikes net (`0000086C`) 101 plus international net (`0000086A`) 239 gives 340 versus published total net 333. Explain CKM; retain directly published totals. Do not impose an invented universal tolerance or require these same differences in future releases.

### M3 — Combined geographical boundary

**Question:** Kan jag summera Malmös och Lunds inflyttningar för att få inflyttningen till deras gemensamma område?

**Reference:** No. Moves between member municipalities are internal to their combined area but included in municipal gross flows. Use a directly published region or suitable origin-destination data excluding internal moves. The problem exists independently of CKM; municipal marginal totals alone cannot identify the required external inflow.

## API controls and future automation

Observed on 2026-10-22:

- Omitting Hushallstyp in a filtered TAB6568 request for Malmö, 2025, household count yielded one observation, 174 327.
- Omitting both Alder and Kon in a filtered TAB6640 request for Malmö, 2025, inflyttningar yielded one observation, 22 434, equal to the explicit total selection.
- TAB1212 with Tid=2025, TAB6640 with ContentsCode=BE0101AU, and TAB1212 with Kon=1+2 each returned HTTP 400, Non-existent value. Do not apply a code from another subject or release silently.
- Search `Flyttningar efter region` returned zero candidates; `flyttningar region` found TAB1212 and TAB6640 among 14 candidates. Search `antal hushåll region` returned 156 candidates on two pages. Simplify empty searches and follow pagination.

Keep `tests/test_scb_skill.py` and its network block intact. Reuse fixtures and checks in `tests/test_pxapi.py` for client request/error handling, cell estimates and JSON-stat conversion when extending actual executable code. Future live checks should be separately opt-in and distinguish source unavailability, changed metadata and revised observations. A fixture test of client code does not establish that an assistant chooses or explains the statistic correctly; retain independent model acceptance for that claim.

## BAS questions

### B1 — Administrative labor-market status versus AKU

**Question:** Hur många arbetslösa personer fanns i Malmö kommun i december 2025, och kan siffran jämföras direkt med AKU:s arbetslöshetstal?

**Reference:** Resolve the current preliminary monthly BAS table and its live metadata. Select Malmö municipality, reference month 2025M12, the table's total-age category matching its population (generally registered residents aged 15–74), and the BAS status `arbetslös`. Report a count in persons, identify the value as preliminary BAS, and explain that it is not directly comparable with AKU's ILO unemployment measure or Arbetsförmedlingen's registered-unemployed count. Check the latest revision notice for the selected observation. This case tests source, population and measure selection; record the API response, current table ID and returned value when performing a live acceptance run rather than freezing a value in this document.
