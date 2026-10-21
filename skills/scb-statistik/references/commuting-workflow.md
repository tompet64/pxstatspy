# SCB commuting workflow

Use this workflow for questions about commuting or employed people by residence and workplace. It is a product-specific guide layered on top of `SKILL.md`'s general search and PxWebApi rules.

## Interpret the question

- Resolve the origin/destination direction explicitly: `bostadskommun` is where a person is registered as resident; `arbetsställekommun` is where their workplace is located.
- Distinguish the number of employed people by residence/workplace from actual trips, trip frequency, distance, route, or mode. The register table does not measure those travel outcomes.
- Determine whether the request is for a single municipality-to-municipality pair, an in-/outflow for one municipality, or a complete origin-destination matrix. Do not assume “pendling till/från” direction; restate the chosen direction briefly.

## Select and validate a table

Search current PxWebApi metadata with separate variants such as `sysselsatta bostadskommun arbetsställekommun`, `pendlare kommun`, and `inpendling utpendling kommun`. Compare the exact measure, age range, method, geography, and period. Prefer current annual register/BAS tables over old RAMS tables when their population and measure fit. Two useful current candidates (both observed for 2020–2024 and updated 2025-11-28; resolve them live every time) illustrate which table to use:

- “Sysselsatta 15–74 år efter bostadens belägenhet i förhållande till arbetsställets belägenhet, kommun och kön. Årligt register” (observed API ID `TAB1828`): use for a municipality's in-commuters, out-commuters, and people who live and work in the same municipality. Its content values state direction explicitly.
- “Sysselsatta 15–74 år efter bostadskommun, arbetsställekommun och kön. Årligt register” (observed API ID `TAB1830`): use for a specific origin-destination pair or a filtered municipality-to-municipality matrix. Here origin is residence municipality and destination is workplace municipality.

If the user's wording is unclear, confirm whether they mean the aggregate commuting to/from a municipality or a specific origin-destination pair. The first table is usually simpler and safer for in-/out-commuting totals.

For a simple all-sex total, verify and select the total category (`1+2`) if available. The origin-destination table has 290 municipalities for each of residence and workplace; selecting all combinations across many years can exceed API limits. Estimate cells before requesting data and filter to requested municipalities, years and breakdowns. Fetch full matrices in bounded chunks only when needed. The aggregate in-/out-commuting table avoids the 290 × 290 matrix when the user only wants a municipality total.

Inspect table notes and SCB's quality documentation before comparing years or calculating change. In the current table metadata, SCB notes disclosure protection (so totals may not equal summed parts), placement of people that cannot be linked to a workplace in their residence municipality, and a 2024 adjustment to the classification of entrepreneurs. Explain any comparability caveat that affects the requested comparison; do not automatically treat all years as fully comparable.

## Report

State the measure as employed people, the age range, origin and destination municipality, reference year(s), unit, table/source, and any method or disclosure caveat. If the question asks about travel volume, mode, distance or route, say this table cannot answer that directly and identify the missing travel data rather than presenting employed-person counts as trips.
