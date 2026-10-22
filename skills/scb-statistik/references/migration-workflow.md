# SCB migration workflow

Use this workflow for registered population movements. It adds migration-specific interpretation to the general search and PxWebApi rules in `SKILL.md`.

## Interpret the requested movement

- Treat migration as a registered change of residence, not a journey, commute, temporary stay or other travel event.
- Resolve the requested geography and direction. In-/out-movement for a municipality is different from an origin-destination flow between two named municipalities.
- Distinguish all in-/out-movements from domestic migration and international immigration/emigration. A net figure can refer to all migration, domestic migration or international migration; retain that scope in the label.
- Prefer a directly published net measure when the table provides it. Do not derive net migration by subtracting or summing components unless the metadata and table notes support that calculation.
- For rates per 1,000 population, verify the exact denominator (for example, mean population by age or at another reference point) and period. Do not divide by year-end population without checking the table's definition.

## Find and select a table

Search separate short variants such as `flyttningar region år`, `inrikes flyttningar kommun`, `invandringar region` and `flyttningsöverskott kommun`. Compare the table title, reference years, geography, breakdowns, contents and update date. Metadata may have large region and age dimensions; inspect only requested labels/codes locally.

As an observed discovery clue in 2026, SCB exposed an annual table for 1997–2024 (“Flyttningar efter region, ålder och kön”; API ID `TAB1212`) and a separate 2025 table with the same title (API ID `TAB6640`). Resolve IDs, periods and codes from live `/tables` and `/metadata` responses every time. The tables split the series at 2025, and their contents and codes differ; do not assume a single code can be used across both. Use an appropriate regional total and all-sex total if requested, and omit optional dimensions only after verifying the default selection and response.

For example, metadata observed on 2026-10-22 used `BE0101AU` for inflyttningar in `TAB1212` and `0000086B` in `TAB6640`. The older table exposed only `1` and `2` for sex; a filtered request omitting `Kon` returned an all-sex aggregate, which must be checked in the response. The newer table exposed `TotSa` for all sexes and `TOT1` for all ages. Do not borrow the commuting tables' sex-total code `1+2`. The newer age domain contains overlapping one-, five- and ten-year classifications and several totals; select one appropriate classification or direct total, never sum the entire domain. Treat these observed codes as examples, not permanent mappings.

Use an annual region table for a municipality's overall flows. Use a table with both origin and destination dimensions only if the request explicitly asks where people moved from or to. A table by birth region describes a different breakdown and is not an origin-destination matrix.

## Check definitions and comparability

- Read every note attached to the selected table and content value. In SCB migration tables, directly published categories can include `Inflyttningar`, `Utflyttningar`, `Invandringar`, `Utvandringar`, `Flyttningsöverskott`, `Invandringsöverskott`, and domestic-movement measures. Which terms and categories appear depends on the table.
- Do not presume that a displayed `Inflyttningar` value means only domestic arrivals. Resolve its definition from metadata and notes; distinguish it from `Inrikes inflyttningar` and `Invandringar`.
- Do not sum municipalities' gross in-/out-movements to estimate flows across the boundary of a combined region. A move between two member municipalities is internal to that region but appears in municipal flows. Prefer a directly published regional measure or an origin-destination selection excluding internal moves. This problem exists independently of disclosure protection; check the measure notes before any geographic aggregation.
- SCB introduced controlled random uncertainty (Cell Key Method, CKM) for reported migration values from reference year 2025 in the observed 2025 table. Its note says reported totals may not equal sums of components and that adding released cells also adds uncertainty. Do not reconstruct or overstate precision of totals from such cells. Check live notes for later years and other tables.
- Historical region codes can reflect the administrative boundaries in force for the event/reference year. Check notes for changes in municipal boundaries and how events are assigned to region before comparing long series.
- Distinguish final annual observations from preliminary current-year/monthly observations. Do not combine them without labeling the status and checking revision notes.

## Report

State whether the values are registered moves or unique movers if the table specifies that distinction, the geographic direction, domestic/international scope, period, unit, breakdowns, table and notes. Label a net value with its component scope (for example, “inrikes flyttningsöverskott”). If the requested origin-destination flow or geographic resolution is not in the selected table, search specifically for that structure instead of substituting municipal in-/out-totals.
