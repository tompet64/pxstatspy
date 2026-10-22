---
name: scb-statistik
description: Find, interpret, validate, and retrieve statistics from Statistics Sweden (SCB) using PxWebApi v2 and SCB geodata sources. Use for Swedish statistical questions about population, household statistics and household income (including disposable income and economic standard), BAS (Befolkningens arbetsmarknadsstatus), employment and unemployment, commuting, migration (flyttningar, immigration and emigration), geography, SCB tables, dimensions, category values, or data extracts.
---

# SCB-statistik

Use this skill to turn a Swedish-language statistical question into a traceable SCB table and a valid PxWebApi v2 selection. Treat semantic interpretation as the skill's job; SCB's `query` parameter is a keyword search, not a semantic search engine.

## Workflow

1. Parse the request into measure/concept, population or statistical unit, geography, time period, frequency, breakdowns, and desired unit. Preserve the user's exact intent; for example, `folkmängd` is a stock at a point in time, while `folkökning` is a change/flow.
2. Search SCB's table list with several short Swedish concept phrases, one at a time. Start with the measure plus a likely dimension (for example, `folkmängd region`), then try meaningful variants such as `befolkning kommun`, `invånare region`, and `antal folkbokförda`. The API `query` is a keyword filter, not a semantic search: a full natural-language sentence can return no rows. A zero-result query is not evidence that SCB lacks the statistic; simplify it and try synonyms.
3. Route geographic requests to the right source. For standard administrative areas and table-shaped statistics, search PxWebApi v2. If the request mentions grid/raster cells, fine spatial resolution, downloadable GIS data, or an area not represented in table metadata, also check SCB's open geodata and geographic-indelning pages. Geodata may be outside PxWebApi; a zero-result table search does not establish that the statistic is unavailable. See `references/geodata-workflow.md` for retrieval and handling. If an appropriate product is paid, commissioned, access-restricted, or otherwise unavailable for open API retrieval, do not seek a workaround or imply it is available; explain the limitation and refer the user to the official provider or ordering page.
4. Screen the compact table-list records before fetching detailed metadata. Compare table title, description, `variableNames`, period coverage, update date and subject path. Use pagination where the table-list response indicates more results. Do not choose on title similarity or update date alone.
5. State the best candidate and why it fits. If a difference would change the measure/table (for example, year-end stock versus monthly population), ask one short clarification. Otherwise proceed with a stated assumption.
6. Fetch detailed metadata only for the leading candidate(s). Some tables have very large category domains (for example, municipality plus DeSO/RegSO areas). Do not print or pass every category label into the conversation. Parse the metadata locally or inspect just the required codes and labels. If that is not practical, compare a more focused table with the same measure, geography and period before proceeding.
7. Build the smallest valid selection matching the request. For an overall total, inspect the table's default selection and test whether optional dimensions can be omitted; PxAPI v2 may apply the default selection and return a single aggregate without category filters. Do not select and sum optional categories unnecessarily. Verify the response dimensions and value before relying on this behavior. If the response still contains a breakdown, use an explicit total category if available or aggregate mutually exclusive categories only after checking notes and disclosure perturbation.
8. Check the estimated cell count, API limits, time coverage, geographic comparability and any break in series. Retrieve data only after the selection is valid.
9. Report the table title and table ID, reference period, update date if available, selected categories and unit. Link to the SCB table/API where possible. Distinguish a data fact from a recommendation about what the user meant.

## Population starter method

For a request about residents/inhabitants or `befolkning`, first determine whether the intended statistic is:

- population stock (`folkmängd`), commonly measured at year-end;
- a current/monthly stock, if the question asks for the latest available value;
- population change (`folkökning`), births, deaths, migration or another flow.

For annual municipality/county population, search the API for current tables matching `folkmängd efter region år`, then verify the table metadata and regional dimension. The portal's familiar table `Folkmängden efter region, civilstånd, ålder och kön` is a useful search clue, not a guaranteed stable API table ID. Prefer a table with the requested time coverage and granularity, and check whether municipal boundaries or regional codes changed during the series. If the user says “latest”, compare the latest reference period and update date, and consider a monthly table; do not silently substitute monthly data for annual year-end values.

Useful paraphrases for candidate discovery include `befolkning`, `invånare`, `folkmängd`, `antal folkbokförda`, plus the requested geography and years. Use these as separate keyword searches rather than expecting one query to understand the full prompt.

## Household starter method

For household questions, distinguish households from residents, families, household members, household type, household size and dwelling/tenure. Use `references/household-workflow.md` for SCB's household-unit definition, the annual year-end tables, coverage limitations, geography breaks and disclosure notes. For household disposable income or comparisons of economic standard, use `references/income-workflow.md` and distinguish household totals from person-level equivalised income.

## Income starter method

For income questions, resolve whether the user means household disposable income, a person's disposable income, or economic standard per consumption unit. Use `references/income-workflow.md` to choose a current SCB table and account for price basis, household population, income definitions and series breaks. Do not treat the preliminary monthly individual-income series as household income.

## BAS starter method

For questions about the registered population's employment, unemployment or other labor-market status, use `references/bas-workflow.md`. Distinguish BAS from AKU's ILO unemployment measure and from Arbetsförmedlingen's registered-unemployed statistics; match age, geography and preliminary/final status before comparing periods.

## Commuting starter method

For commuting questions, distinguish the count of employed people classified by residence and workplace municipality from actual journeys, trip frequency, distance, route, or mode. Use `references/commuting-workflow.md` to resolve direction, choose the current annual register table, account for method notes and avoid oversized origin-destination extracts. Treat table IDs such as `TAB1830` as discovery clues only; resolve current metadata on each task.

## Migration starter method

For migration questions, distinguish registered population movements from travel or temporary mobility. Resolve whether the user means all in-/out-movements, domestic moves, immigration/emigration, or net migration. Use `references/migration-workflow.md` for table discovery, component meanings, time-series splits and disclosure notes. Resolve current table IDs and value codes from live metadata; do not assume codes match between adjacent tables.

## API and client rules

- Read `references/PxAPI-2.yml` for the pinned API contract. It is an official OpenAPI snapshot; the live API and current table metadata take precedence when they differ.
- The usual SCB v2 base URL is `https://statistikdatabasen.scb.se/api/v2`. Confirm current paths and parameter names in the spec. In particular, the GET data filter is `valuecodes` (lowercase) as documented in this snapshot; POST accepts a JSON selection body.
- If no values are specified, the data endpoint applies the table's default selection. Inspect `/tables/{id}/defaultselection` when unclear; omission may yield an aggregate for optional variables. Verify the response shape rather than assuming omission means either “all categories” or “total.”
- Use `/tables` for table discovery, `/tables/{id}` for compact table metadata, `/tables/{id}/metadata` for dimensions/category values and notes, and `/tables/{id}/data` for observations. Pass language explicitly when useful. Handle pagination and response errors; don't assume the first page contains all candidates.
- `pxstatspy` is an optional third-party client adapter, not an SCB product and not a semantic search engine. Inspect whether it is installed and which version/API it exposes. Use it only for operations it supports cleanly, and validate the request/response against the OpenAPI contract. Do not add it as a mandatory dependency or install it automatically. If absent or mismatched, call the documented REST API directly using an available HTTP client.
- Do not rely on a remembered table ID or a code copied from an old portal. Resolve it from current API metadata on each task.
- Respect API limits stated in current SCB documentation/configuration. Minimize requests, avoid fetching all categories unless required, and split large selections only when necessary.
- Do not imply that all SCB data are delivered through PxWebApi. For raster/geodata requests, use SCB geodata services or downloadable files and follow `references/geodata-workflow.md`. Explain when finer resolution or a custom geography requires commissioned data. Do not silently aggregate, spatially join or sum grid cells without checking the measure, boundary/version and disclosure protection.

## Maintain statistical product coverage

Whenever adding support for a statistical product or subject, update `SKILL.md` in the same change, even when extending an existing guide. Keep the description's discovery terms, the relevant starter method or routing instruction, and the reference list aligned with the supported scope; link any new guide directly here. Do not treat a standalone reference file as a completed product addition. Verify current metadata and record the statistical unit, population, geography, reference period, units, comparability and disclosure limitations in the guide. Add or update a realistic statistical acceptance case, run the existing offline structural validation, and report statistical checks separately from structural checks. Keep new live API checks separate from the offline validator.

## References

- `references/semantic-workflow.md` — candidate comparison and worked population example.
- `references/geodata-workflow.md` — when to search SCB geodata, supported access patterns, and ZIP/GeoPackage handling.
- `references/bas-workflow.md` — BAS status categories, table selection, age/geography alignment, revision handling and distinction from AKU.
- `references/commuting-workflow.md` — candidate identification, origin/destination interpretation, query sizing and comparability checks for commuting statistics.
- `references/household-workflow.md` — household-unit interpretation, table candidates, coverage, geography, and disclosure protection.
- `references/income-workflow.md` — household disposable income, economic standard, price basis, population and income-series comparability.
- `references/migration-workflow.md` — domestic/international migration, in-/outflow and net-migration semantics, table selection and comparability.
- `references/pxstatspy-assessment.md` — when the optional Python library is useful.
- `references/OpenAPI-source.md` — provenance and version details for the included contract.
- `references/PxAPI-2.yml` — official OpenAPI contract snapshot.
