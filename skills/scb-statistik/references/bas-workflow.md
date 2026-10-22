# SCB BAS workflow

Use this guide for Befolkningens arbetsmarknadsstatus (BAS), a register-based statistical product about the labor-market status of the registered population. Resolve current table IDs, categories and definitions from PxWebApi v2 on every request.

## Distinguish BAS from other unemployment measures

- BAS describes the registered population, generally ages 15–74, using administrative data. Its six mutually exclusive statuses are employed, unemployed, studying, pensioner, sick and other. Status assignment is hierarchical; a person who meets more than one criterion receives one principal status. Check the current quality declaration for the exact definitions.
- BAS unemployment is not interchangeable with the unemployment measure from the Labour Force Survey (AKU). AKU is the sample survey used for the internationally comparable ILO unemployment measure. If the user asks for the official unemployment rate, ILO definition, or international comparison, route to AKU unless they explicitly mean BAS. Arbetsförmedlingen's count of registered unemployed is also a different measure.
- Explain which source and definition a rate uses. Do not label a BAS count or rate simply as “unemployment” when that could imply AKU.

## Find the right table

Search with short phrases, one at a time, such as `arbetsmarknadsstatus region månad`, `BAS sysselsatta region`, `arbetslösa region`, or `arbetsmarknadsstatus utbildningsnivå region`. Inspect additional candidates when the question asks for income source, work-related income level, industry, education, or employment status. Compare the title, statistical unit, dimensions, period, reference frequency, update date, prelim/final status and notes.

The current table list has separate products for preliminary monthly status, final monthly status, annual final status, and derived key indicators. For example, the preliminary monthly table “Arbetsmarknadsstatus efter region, kön, ålder och födelseregion” and a monthly variant with education are useful discovery leads. Do not assume these titles, IDs or coverage remain unchanged; verify the current metadata. Prefer a published rate table when the user asks for a rate, rather than calculating one from counts with a mismatched denominator.

## Align population, geography and period

- Check the age population in the table. BAS commonly describes the registered population aged 15–74, while headline employment-rate and unemployment-rate indicators may refer to ages 20–65. Never mix an age-specific numerator and denominator.
- Resolve whether the requested region means place of residence or workplace; do not assume “sysselsatta efter region” uses the user's intended geography. Read the table description and notes.
- Distinguish a monthly status from an annual status. The preliminary monthly statistics are suited to recent developments; final monthly or annual statistics may be better for finalized comparisons. Do not splice preliminary and final series without explaining the transition and checking definitions.
- Check whether the user wants number of people, share/rate, employment, unemployment, or everyone outside the labor force. “Outside the labor force” is not automatically the sum of any arbitrary status categories; use a published aggregate or verify mutually exclusive categories and the denominator before deriving it.

## Handle revisions and uncertainty

- BAS is under development and preliminary values can be revised. Check the table's current notes and SCB's revision notices before describing trends. A May 2026 notice revised preliminary observations from January 2020 through February 2026; treat this as a dated example, not a permanent rule.
- SCB applies statistical disclosure protection. Published totals may not equal sums of displayed subgroups. Prefer direct totals and published rates; do not infer an API error from non-additivity.
- If a table is preliminary, say so. Report the reference month or year, update date, table title and current ID, exact age/geography/status selections, unit, and relevant revision or comparability caveat.

## Useful official sources

- [SCB: Befolkningens arbetsmarknadsstatus (BAS)](https://www.scb.se/AM0210)
- [SCB: BAS FAQ](https://www.scb.se/hitta-statistik/statistik-efter-amne/arbetsmarknad/utbud-av-arbetskraft/befolkningens-arbetsmarknadsstatus/produktrelaterat/fordjupad-information/faq/)
- [SCB: revision of preliminary BAS statistics](https://www.scb.se/contentassets/f7fc10dacfef4075a98be3b56724f7e6/revidering-i-preliminar-bas-statistik.pdf)
