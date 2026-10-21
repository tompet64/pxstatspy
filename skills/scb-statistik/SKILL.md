---
name: scb-statistik
description: Find, interpret, validate, and retrieve statistics from Statistics Sweden (SCB) using PxWebApi v2. Use for Swedish statistical questions that need SCB tables, dimensions, category values, or data extracts, especially population statistics.
---

# SCB-statistik

Use this skill to turn a Swedish-language statistical question into a traceable SCB table and a valid PxWebApi v2 selection. Treat semantic interpretation as the skill's job; SCB's `query` parameter is a keyword search, not a semantic search engine.

## Workflow

1. Parse the request into measure/concept, population, geography, time period, frequency, breakdowns, and desired unit. Preserve the user's exact intent; for example, `folkmängd` is a stock at a point in time, while `folkökning` is a change/flow.
2. Search SCB's table list with several short Swedish concept phrases, one at a time. Start with the measure plus a likely dimension (for example, `folkmängd region`), then try meaningful variants such as `befolkning kommun`, `invånare region`, and `antal folkbokförda`. The API `query` is a keyword filter, not semantic search: a full natural-language sentence can return no rows. A zero-result query is not evidence that SCB lacks the statistic; simplify it and try synonyms.
3. Screen the compact table-list records before fetching detailed metadata. Compare table title, description, `variableNames`, period coverage, update date and subject path. Use pagination where the table-list response indicates more results. Do not choose on title similarity or update date alone.
4. State the best candidate and why it fits. If a difference would change the measure/table (for example, year-end stock versus monthly population), ask one short clarification. Otherwise proceed with a stated assumption.
5. Fetch detailed metadata only for the leading candidate(s). Some tables have very large category domains (for example, municipality plus DeSO/RegSO areas). Do not print or pass every category label into the conversation. Parse the metadata locally or inspect just the required codes and labels. If that is not practical, compare a more focused table with the same measure, geography and period before proceeding.
6. Build the smallest valid selection matching the request. For an overall total, inspect the table's default selection and test whether optional dimensions can be omitted; PxAPI v2 may apply the default selection and return a single aggregate without category filters. Do not select and sum optional categories unnecessarily. Verify the response dimensions and value before relying on this behavior. If the response still contains a breakdown, use an explicit total category if available or aggregate mutually exclusive categories only after checking notes and disclosure perturbation.
7. Check the estimated cell count, API limits, time coverage, geographic comparability and any break in series. Retrieve data only after the selection is valid.
8. Report the table title and table ID, reference period, update date if available, selected categories and unit. Link to the SCB table/API where possible. Distinguish a data fact from a recommendation about what the user meant.

## Population starter method

For a request about residents/inhabitants or `befolkning`, first determine whether the intended statistic is:

- population stock (`folkmängd`), commonly measured at year-end;
- a current/monthly stock, if the question asks for the latest available value;
- population change (`folkökning`), births, deaths, migration or another flow.

For annual municipality/county population, search the API for current tables matching `folkmängd efter region år`, then verify the table metadata and regional dimension. The portal's familiar table `Folkmängden efter region, civilstånd, ålder och kön` is a useful search clue, not a guaranteed stable API table ID. Prefer a table with the requested time coverage and granularity, and check whether municipal boundaries or regional codes changed during the series. If the user says “latest”, compare the latest reference period and update date, and consider a monthly table; do not silently substitute monthly data for annual year-end values.

Useful paraphrases for candidate discovery include `befolkning`, `invånare`, `folkmängd`, `antal folkbokförda`, plus the requested geography and years. Use these as separate keyword searches rather than expecting one query to understand the full sentence.

## API and client rules

- Read `references/PxAPI-2.yml` for the pinned API contract. It is an official OpenAPI snapshot; the live API and current table metadata take precedence when they differ.
- The usual SCB v2 base URL is `https://statistikdatabasen.scb.se/api/v2`. Confirm current paths and parameter names in the spec. In particular, the GET data filter is `valuecodes` (lowercase) as documented in this snapshot; POST accepts a JSON selection body.
- If no values are specified, the data endpoint applies the table's default selection. Inspect `/tables/{id}/defaultselection` when unclear; omission may yield an aggregate for optional variables. Verify the response shape rather than assuming omission means either “all categories” or “total.”
- Use `/tables` for table discovery, `/tables/{id}` for dimensions and category values, and `/tables/{id}/data` for observations. Pass language explicitly when useful. Handle pagination and response errors; don't assume the first page contains all candidates.
- `pxstatspy` is an optional third-party client adapter, not an SCB product and not a semantic search engine. Inspect whether it is installed and which version/API it exposes. Use it only for operations it supports cleanly, and validate the request/response against the OpenAPI contract. Do not add it as a mandatory dependency or install it automatically. If absent or mismatched, call the documented REST API directly using an available HTTP client.
- Do not rely on a remembered table ID or a code copied from an old portal. Resolve it from current API metadata on each task.
- Respect API limits stated in current SCB documentation/configuration. Minimize requests, avoid fetching all categories unless required, and split large selections only when necessary.

## References

- `references/semantic-workflow.md` — candidate comparison and worked population example.
- `references/pxstatspy-assessment.md` — when the optional Python library is useful.
- `references/OpenAPI-source.md` — provenance and version details for the included contract.
- `references/PxAPI-2.yml` — official OpenAPI contract snapshot.
