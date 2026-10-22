# SCB household statistics workflow

Use this guide for statistics where the unit is a household or where people are classified by household membership. Resolve current IDs and category codes from live PxWebApi v2 metadata; the IDs below are discovery clues observed on 2026-10-22.

## Define the statistical unit

- Treat a household as a dwelling household, not as a family, a group of relatives, a housing unit, or an owner/tenant group. SCB's household statistics generally define one household from people registered at the same dwelling.
- Distinguish measures about **households** from measures about **people living in households**. A household count, resident population, number of people with household information, coverage share, and average household size are different measures.
- Ask whether the request concerns household size/type, people in a household position, dwelling/tenure, or all households. Tables with similar titles may use different household-type definitions; check the selected table's notes.
- Do not imply the household register represents every resident equally. People not registered to a physical dwelling may lack household information; report the table's coverage measure and caveat when relevant.

## Find and choose a table

Search separate short phrases such as `antal hushåll region`, `hushållstyp kommun`, `personer per hushåll region`, `hushållsstorlek län`, and `hushåll barn region`. Compare table description, dimensions, period, reference date, unit and notes before choosing.

Useful observed candidates:

- **TAB4374**, “Antal personer och hushåll samt antal personer per hushåll efter region. År 2011–2025”: a compact starting point for population, people with household data, share with household data, household count, and persons per household by region and year. It is annual, measured on 31 December; all five measures are stock measures. Its observed latest update was 2026-03-24.
- **TAB6568**, “Antal hushåll per region efter hushållstyp. År 2011–2025”: use when the question specifically concerns household type. Its observed selectable types are `SBMB` (sammanboende med barn), `SBUB` (sammanboende utan barn), `ESMB` (ensamstående med barn), `ESUB` (ensamstående utan barn), and `OVRIGA` (övriga hushåll), plus `TOTALT`. Do not claim that `OVRIGA` is split by presence of children in this table. Its region dimension also includes DeSO and RegSO areas, so restrict selection to the requested geography and estimate cells before extracting.
- Search for more focused household-size, children, age-of-youngest-child, tenure or dwelling tables only when those attributes are part of the question. Do not substitute household-type statistics for housing tenure or for individual-level demographics.

Resolve the table and exact variable/value codes from live API metadata each time. Table titles and IDs can change. For household totals, inspect `defaultselection` and test omission of optional dimensions instead of selecting and summing every category.

## Check coverage, geography and time

- Check which people can be assigned to households and whether the selected measure uses only those with household information. TAB4374 explicitly notes that a household cannot be created for people not registered at a physical property; its average household size is based on people with household information.
- Treat `31 December` figures as year-end stocks, not annual flows or annual averages. Household counts are not interchangeable with population counts.
- Household-type categories are classifications of household composition. In TAB6568, “child” can be defined regardless of age when a person lives with at least one parent and lacks a child or partner in the same household; verify the live note.
- For DeSO or RegSO comparisons, check the geography version. The observed TAB6568 notes say new DeSO 2025 and RegSO 2025 versions apply from reference year 2024, replacing DeSO 2018 and RegSO 2020, and that some areas are not comparable over time. Prefer municipality/county totals for longer comparisons unless the user explicitly needs small areas and the codes can be reconciled.
- Check the live metadata and table notes for changes in household construction, classification and disclosure treatment. Do not assume all years are fully comparable just because they are in one table.

## Handle disclosure protection

The observed 2025 household tables report Cell Key Method (CKM) controlled random uncertainty from reference year 2025 onward. SCB warns that published totals may not equal the sum of published components and that summing released cells also accumulates uncertainty. Therefore:

- Prefer a directly published total or percentage when available.
- Do not reconstruct totals from household-type, geography or other components unless SCB documentation explicitly supports it.
- If a user requests a sum or derived percentage, check whether the underlying values are disclosure-perturbed and describe the limitation. Avoid presenting an arithmetic result from perturbed cells as an exact official total.

Older reference years may use different disclosure-control methods. Check the table's notes for the selected period.

## Report

State the statistical unit, measure, geography, reference date, period, unit, table title/ID and update date. Explain household coverage or classification if it affects interpretation. Note that household statistics describe registered dwelling households and do not identify vehicle ownership, disposable income or family relationships beyond the table's own definition.
