# SCB household income workflow

Use this guide for household income, especially disposable income, and for related questions about economic standard. Resolve current table IDs, periods, value codes and notes from live PxWebApi v2 metadata; IDs below are discovery clues observed on 2026-10-22.

## Choose the income concept

- **Disposable income for households** is the sum of all household members' taxable and tax-free incomes, minus taxes and other negative transfers. It is a household-level amount; larger households may have higher totals simply because more people contribute.
- **Economic standard** (equivalised disposable income) divides household disposable income by a consumption-unit weight based on household composition. SCB reports this measure for people, assigning members of a household the same standard. Use it when comparing living standards across household sizes and types.
- Do not treat individual monthly disposable income as household income. SCB's preliminary monthly series is individual income and excludes capital income.
- Resolve whether the user wants mean, median, a distribution/fractile, income source, a household type, or the economic standard of people. These answer different questions.

## Find and select a table

Search separate short phrases such as `disponibel inkomst hushåll region`, `ekonomisk standard region hushållstyp`, `inkomstfördelning hushåll fraktiler`, and `inkomststruktur disponibel inkomst`. Check the live table listing and metadata rather than relying on remembered IDs.

Observed candidates:

- **TAB1492**, “Disponibel inkomst för hushåll efter region, hushållstyp och ålder. Fasta priser. År 2011–2024”: starting point for household mean/median disposable income by region, household type, and age of the household reference person. Values are in thousands of kronor at the latest year's price level. Age refers to the adult with the highest total earned income. The table has mean and median in kronor, plus measures expressed as a percentage of all households at national level.
- **TAB1500**, “Fördelning inkomster för hushåll (fraktiler) efter region. Fasta priser. År 2011–2024”: use for income-distribution thresholds/fractiles and income concentration. It concerns total household income, so household size affects a household's position in the distribution.
- **TAB1123**, “Ekonomisk standard för personer efter region, hushållstyp och ålder. Fasta priser. År 2011–2024”: use for person-level comparisons that account for household composition. It includes mean/median per consumption unit and shares below/above thresholds based on the national median.
- The SCB subject area also publishes riks-level household disposable income by household type back to 1991 and other income distribution/structure tables. Use those when the requested scope or period calls for them; do not substitute a national-only table for a requested regional comparison.
- Monthly disposable-income tables are a separate, preliminary product about individuals, exclude capital income, and may use modelled components. Use them only when the user asks for monthly individual income.

The IDs, years and table structure are search leads, not constants. Resolve current metadata and data notes for every request. For a requested current-price amount, check whether SCB exposes a suitable current-price table or value in the live PxWebApi; do not convert fixed-price values or assert that a current-price API series exists without verifying it.

## Check definitions, population and comparability

- TAB1492 defines disposable income as taxable and tax-free incomes minus taxes and negative transfers. Its values include realized capital gains/losses. State this when it matters to interpretation.
- The household-income population is restricted, among other things, to **full-year households**: adult members must have been registered in Sweden at both the beginning and end of the income year. Use the quality declaration for exact coverage and exclusions.
- The income series includes wage and pension income from other Nordic countries from income year 2011. SCB revised 2011–2019 alongside the 2020 publication; the values for 2011–2014 have weaker Nordic-income coverage, particularly for Norway.
- From publication of income year 2021, SCB accounts for children who alternate between parents' homes and revised 2011–2020. The model for simulating alternate residence was updated for 2022, creating a specific break for children with alternate residence. Check the current quality declaration before describing a long-term trend.
- Fixed-price amounts allow purchasing-power comparisons over time. Report the base-price year specified by the table. Percent-of-national values are relative comparison measures, not kronor.
- Economic standard uses a consumption-unit scale and is a per-person measure. Do not divide a household amount by household member count or describe the resulting average as SCB's official economic standard.
- SCB applies statistical disclosure protection. Published totals may not equal sums of published subgroups. Prefer direct totals and rates; do not reconstruct total income by summing categories unless the metadata and notes support it.

## Report

Name the concept and statistical unit (household income or person-level economic standard), measure (mean/median/fractile), geography, household grouping, income year, price basis/base year and unit. Give the table title and current table ID, and link to the official SCB table. State relevant population limits and any comparability caveat. Do not imply that disposable income measures wealth, consumption, or transport affordability directly.
