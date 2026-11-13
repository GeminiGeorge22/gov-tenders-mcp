# Gov Tenders & Grants MCP: SAM.gov, Grants.gov, USAspending, UK, EU TED

**Public-sector opportunity data as MCP tools**: US federal contract opportunities (SAM.gov), federal grants (Grants.gov), federal awards (USAspending), UK tenders (Find a Tender, Contracts Finder), EU tenders (TED) and city council meeting minutes and agendas (Legistar). Every row carries its official source link.

Hosted remote MCP server (Streamable HTTP) on [Apify's MCP gateway](https://mcp.apify.com). **Client config plus a small open-source stdio proxy server. No Actor source.**

**Disclosure:** I maintain these Actors on Apify Store (publisher `publicrecords`).

## MCP endpoint

```
https://mcp.apify.com/?tools=publicrecords/govcon-opportunity-feed,publicrecords/grants-gov-scraper,publicrecords/usaspending-awards-scraper,publicrecords/uk-find-a-tender-scraper,publicrecords/eu-ted-notices-scraper,publicrecords/legistar-meetings-scraper
```

Auth: sign in with Apify (OAuth) when your client prompts, or send an Apify API token as a Bearer token. Runs are billed per result on your own Apify account; see each Store page for current pricing.

## Billing

- The tools call paid Actors on Apify Store, published by `publicrecords`. Each call starts a run on **your own Apify account**, using your own Apify token (or Apify OAuth sign-in).
- Apify bills each run to that account on a pay-per-event basis: one event when the run starts, plus one event per result row returned. Nothing is billed to anyone else, and there is no separate subscription.
- This repo's code and config are free (MIT). The local proxy server keeps no data and no tokens: it reads your `APIFY_TOKEN` from the environment, sends it only in the `Authorization` header to Apify's MCP gateway, and never logs or stores it. Run results are stored in your Apify account, under your Apify data-retention settings.
- For current prices, see each Actor's Store page (Pricing tab). Prices are not repeated here because they can change, and the Store page is the source of truth:
  - [publicrecords/govcon-opportunity-feed](https://apify.com/publicrecords/govcon-opportunity-feed)
  - [publicrecords/grants-gov-scraper](https://apify.com/publicrecords/grants-gov-scraper)
  - [publicrecords/usaspending-awards-scraper](https://apify.com/publicrecords/usaspending-awards-scraper)
  - [publicrecords/uk-find-a-tender-scraper](https://apify.com/publicrecords/uk-find-a-tender-scraper)
  - [publicrecords/eu-ted-notices-scraper](https://apify.com/publicrecords/eu-ted-notices-scraper)
  - [publicrecords/legistar-meetings-scraper](https://apify.com/publicrecords/legistar-meetings-scraper)
- Use `maxItems` on each tool to cap how many rows (and so how many results) a call can return.

## Tools

| Actor | Store page | MCP tool | What it returns |
|---|---|---|---|
| **Federal Contract Opportunities** | [publicrecords/govcon-opportunity-feed](https://apify.com/publicrecords/govcon-opportunity-feed) | `publicrecords--govcon-opportunity-feed` | SAM.gov and Grants.gov notices in one feed, with new-since-last-run deltas and source links. |
| **Grants.gov Search** | [publicrecords/grants-gov-scraper](https://apify.com/publicrecords/grants-gov-scraper) | `publicrecords--grants-gov-scraper` | Open federal grants: opportunity, agency, deadline, eligibility and source link. |
| **USAspending Awards** | [publicrecords/usaspending-awards-scraper](https://apify.com/publicrecords/usaspending-awards-scraper) | `publicrecords--usaspending-awards-scraper` | Federal contract and grant awards by agency, recipient, NAICS and period, with award IDs and source links. |
| **UK Find a Tender & Contracts Finder** | [publicrecords/uk-find-a-tender-scraper](https://apify.com/publicrecords/uk-find-a-tender-scraper) | `publicrecords--uk-find-a-tender-scraper` | UK government tender notices with buyer, value, deadline and source link. |
| **EU TED Tenders** | [publicrecords/eu-ted-notices-scraper](https://apify.com/publicrecords/eu-ted-notices-scraper) | `publicrecords--eu-ted-notices-scraper` | EU procurement notices from Tenders Electronic Daily with buyer, CPV, value, deadline and source link. |
| **Legistar Council Meetings** | [publicrecords/legistar-meetings-scraper](https://apify.com/publicrecords/legistar-meetings-scraper) | `publicrecords--legistar-meetings-scraper` | City council meetings, agendas, minutes and documents for any city on Legistar, with source link. |

The Apify gateway also exposes helper tools to read run results (`get-actor-run`, `get-dataset-items`).

### Tool reference (inputs and outputs)

Inputs below are copied from [`tools.json`](tools.json), which is generated from each Actor's public input schema (`npm run build:tools`). No input is required unless marked. Outputs are the fields of the rows each run writes to its Apify dataset, from each Actor's published dataset schema; not every field is filled on every row, and each row has a `recordType` saying what kind of row it is. Every tool returns those rows as structured JSON plus the run and dataset IDs for paging the full dataset.

#### `publicrecords--govcon-opportunity-feed`

Federal Contract Opportunities: SAM.gov + Grants.gov in one. Use when you need new US federal contract opportunities (SAM.gov) and grants (Grants.gov) in one feed, optionally only items new since the last run. Store page: <https://apify.com/publicrecords/govcon-opportunity-feed>

**Inputs**

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `mode` | string (one of: `monitor`, `snapshot`, `enrich`, `rehashState`) | no | `"monitor"` | Mode |
| `sources` | array of string | no | `["sam", "grants"]` | Sources. Allowed values: sam, grants. SAM defaults to daily Data Services CSV (samBulk=true, no key). Grants.gov needs no key. |
| `samApiKey` | string | no | — | SAM.gov API key. Optional when samBulk=true. Required only for API enrich (resourceLinks) / monitor checks / samBulk=false. Personal public API key from sam.gov (Account Details → Request API Key). Free. Stored encrypted; never logged. |
| `naics` | array of string | no | `[]` | NAICS codes. 6-digit or prefix, e.g. 541512 or 5415. |
| `agencies` | array of string | no | `[]` | Agencies. Names or codes, e.g. Department of Defense, DHS, EPA. |
| `keywords` | array of string | no | `[]` | Keywords. Free-text keywords matched against opportunity titles. |
| `setAside` | array of string | no | `[]` | Set-aside types. SB, 8A, HUBZone, SDVOSB, WOSB, NONE |
| `states` | array of string | no | `[]` | Place of performance states. Two-letter US state codes for place of performance (contracts). |
| `noticeTypes` | array of string | no | `[]` | Notice types. Filter by notice type, e.g. solicitation, award_notice, grant_posted. |
| `postedWithinDays` | integer (min 1, max 365) | no | `14` | Posted within N days. Only include opportunities posted within the last N days. Declared product default is **14 days** (Thursday product window). |
| `deadlineAfter` | string | no | — | Deadline after. Defaults to today; excludes expired opportunities. |
| `since` | string | no | — | Since date. ET calendar date (YYYY-MM-DD) or RFC3339. Selects HF delta shards with eventDate >= since. Optional for snapshot (serves latest snapshot). For monitor, defaults to a 7-day lookback ending at the latest published delta date. Example: "2026-09-21" |
| `includeDescriptions` | boolean | no | `false` | Include full descriptions. When true, fetch full grant synopses (slower). |
| `includeContacts` | boolean | no | `false` | Include official contacts. When true, include SAM point-of-contact fields. |
| `enrichTopN` | integer (min 1, max 20) | no | `5` | Incumbents per opportunity (enrich mode). Category leaders (or predecessors) per opportunity in enrich mode. |
| `maxItems` | integer (min 1, max 50000) | no | `10000` | Max items. Hard cap on opportunity rows returned (and billed) after sort by postedDate desc → Sol#. Default 10000. Under the default 14d window, Active matches often exceed 10k — runs emit CAP_TRUNCATED + CAP_COVERAGE.covered/total (never silent). Cap always drops the oldest rows. |
| `samBulk` | boolean | no | `true` | Use SAM Data Services daily CSV (bulk). Ignored on the public Actor (v1.2 HF serve). Retained for input compatibility; private producer uses SAM Data Services CSV. |
| `samEnrichLimit` | integer (min 0, max 50) | no | `5` | SAM API enrich budget. Ignored on the public Actor (v1.2 HF serve). Retained for input compatibility. |
| `incumbentMode` | string (one of: `category_leaders`, `predecessor`) | no | `"category_leaders"` | Enrich join mode. category_leaders (default): top USASpending recipients by agency×NAICS. predecessor: true prior-awardee/recompete join (stub until PREDECESSOR_JOIN.md ships). Proof schedules must keep default. |

**Output fields** (one dataset row per result)

| Field | Type | Notes |
|---|---|---|
| `recordType` | string | opportunity \| delta \| incumbent \| status \| error |
| `opportunityKey` | string |  |
| `kind` | string |  |
| `noticeType` | string |  |
| `title` | string |  |
| `agency` | object |  |
| `naics` | array |  |
| `setAside` | string |  |
| `postedAt` | string |  |
| `deadlineAt` | string |  |
| `url` | string |  |
| `changeType` | string |  |
| `sources` | array |  |
| `recordHash` | string |  |
| `sourceIds` | object | opportunity rows: the SAM notice ID and/or Grants.gov ID. |
| `samNoticeIds` | array | opportunity rows: every SAM NoticeId merged under this opportunity. |
| `description` | string | opportunity rows. |
| `psc` | string | opportunity rows: product/service code. |
| `placeOfPerformance` | object | opportunity rows. |
| `updatedAt` | string | opportunity rows. |
| `lastActivityAt` | string | opportunity rows: latest notice activity in the cluster. |
| `estimatedValue` | object | opportunity rows: amount, currency (USD) and basis (stated \| ceiling \| award_floor \| award_ceiling) |
| `aln` | array | opportunity rows (grants) |
| `contacts` | array | opportunity rows. |
| `attachmentUrls` | array | opportunity rows |
| `sourceUrl` | string | opportunity rows. |
| `retrievedAt` | string | opportunity rows. |
| `sourceVersion` | string | opportunity rows. |
| `cancellationConfirmed` | boolean | opportunity rows: set when the SAM type label confirms a cancellation |
| `previous` | object | delta rows: the changed fields before the change |
| `current` | object | delta rows: the changed fields after the change. |
| `detectedAt` | string | delta rows. |
| `provenance` | string |  |
| `backfillInput` | string |  |
| `recipientName` | string | incumbent rows |
| `recipientUei` | string | incumbent rows |
| `awardCount` | number | incumbent rows |
| `totalObligated` | number | incumbent rows (USD) |
| `lastAwardDate` | string | incumbent rows |
| `sampleAwardIds` | array | incumbent rows: up to 3 |
| `matchBasis` | string | incumbent rows: agency+naics \| naics_only \| agency_only |

#### `publicrecords--grants-gov-scraper`

Grants.gov Search API: federal grants list with deadlines. Use when you need open US federal grant opportunities from Grants.gov with agency, deadline and eligibility. Store page: <https://apify.com/publicrecords/grants-gov-scraper>

**Inputs**

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `keyword` | string | no | `""` | Keyword. Search keyword (e.g. education, cybersecurity). Leave empty for all. |
| `agency` | string | no | `""` | Agency. Mapped to API body agencies (e.g. HHS, EPA, NSF). |
| `status` | string | no | `"forecasted\|posted"` | Opportunity status. Mapped to API oppStatuses. Use forecasted\|posted\|closed\|archived or pipe-joined. |
| `postedWithinDays` | integer (min 1, max 3650) | no | — | Posted within N days. Server-side filter mapped to search2 dateRange (posted-date lookback). Verified values: 3,7,14,21,28,35,42,49,56,90,180,365. Omit for no posted-date filter. |
| `openDateFrom` | string | no | — | Open date from. Client-side filter on openDate. Accepts MM/DD/YYYY or YYYY-MM-DD. Inclusive. |
| `openDateTo` | string | no | — | Open date to. Client-side filter on openDate. Inclusive. |
| `closeDateFrom` | string | no | — | Close date from. Client-side filter on closeDate. Inclusive. Useful for 'still open' windows. |
| `closeDateTo` | string | no | — | Close date to. Client-side filter on closeDate. Inclusive. |
| `maxItems` | integer (min 1, max 5000) | no | `50` | Max items. Cap on opportunity rows (deduped by opportunityId). |
| `fetchDetails` | boolean | no | `false` | Fetch opportunity details. Hydrate synopsis via fetchOpportunity. |

**Output fields** (one dataset row per result)

| Field | Type | Notes |
|---|---|---|
| `recordType` | string | opportunity \| error |
| `opportunityId` | string or null |  |
| `opportunityNumber` | string or null |  |
| `title` | string or null |  |
| `agencyCode` | string or null |  |
| `agencyName` | string or null |  |
| `openDate` | string or null |  |
| `closeDate` | string or null |  |
| `oppStatus` | string or null |  |
| `aln` | string or null |  |
| `detailUrl` | string or null |  |
| `error` | string or null |  |
| `docType` | string or null |  |
| `httpStatus` | integer or null |  |
| `query` | string or null |  |
| `synopsis` | string or null | Only when fetchDetails is on. |
| `fundingInstrument` | string or null | Only when fetchDetails is on. |
| `costSharing` | string or null | Only when fetchDetails is on. |

#### `publicrecords--usaspending-awards-scraper`

USAspending API Awards: federal contract and grant awards. Use when you need who won US federal contracts and grants and for how much (USAspending), by agency, recipient, NAICS or period. Store page: <https://apify.com/publicrecords/usaspending-awards-scraper>

**Inputs**

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `keywords` | array of string | no | `[]` | Keywords. Mapped to filters.keywords. Leave empty for broad recent awards; prefer agency/recipient/date. |
| `naics` | array of string | no | `[]` | NAICS codes. Mapped to filters.naics_codes. |
| `agency` | string | no | `""` | Awarding agency (toptier name). Toptier awarding agency name → filters.agencies[{type:awarding,tier:toptier,name}]. Example: Department of Defense. |
| `recipient` | string | no | `""` | Recipient name. Mapped to filters.recipient_search_text (array). Example: Boeing. Verified shape requires an array of strings. |
| `awardTypes` | array of string | no | `["A", "B", "C", "D"]` | Award type codes. Default A,B,C,D (contracts/IDVs). Avoid assistance codes that may list individuals. |
| `startDate` | string | no | — | Start date. YYYY-MM-DD. Default: today − 30 days (recent awards window). |
| `endDate` | string | no | — | End date. YYYY-MM-DD. Default: today. |
| `subawards` | boolean | no | `false` | Subawards mode. Pass subawards flag to search (default false). |
| `fetchDetails` | boolean | no | `false` | Fetch award details. Hydrate via GET /awards/{generated_internal_id}/. |
| `maxItems` | integer (min 1, max 5000) | no | `50` | Max items. Cap on award rows (deduped by awardId\|generatedInternalId). |

**Output fields** (one dataset row per result)

| Field | Type | Notes |
|---|---|---|
| `recordType` | string | award \| error |
| `awardId` | string or null |  |
| `generatedInternalId` | string or null |  |
| `recipientName` | string or null |  |
| `recipientUei` | string or null |  |
| `awardAmount` | number or null |  |
| `startDate` | string or null |  |
| `endDate` | string or null |  |
| `awardingAgency` | string or null |  |
| `detailUrl` | string or null |  |
| `error` | string or null |  |
| `description` | string or null |  |
| `awardType` | string or null | May be empty. |
| `subaward` | boolean |  |

#### `publicrecords--uk-find-a-tender-scraper`

Find a Tender & Contracts Finder: UK government tenders. Use when you need UK public-sector tenders from Find a Tender and Contracts Finder with buyer, value and deadline. Store page: <https://apify.com/publicrecords/uk-find-a-tender-scraper>

**Inputs**

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `keywords` | array of string | no | `[]` | Keywords. Case-insensitive match on tender title, description, or buyer name (client-side). |
| `cpv` | array of string | no | `[]` | CPV codes. Match when tender.items[].classification.id starts with any code (client-side). |
| `noticeType` | array of string | no | `[]` | Notice stages. Mapped to API stages: planning \| tender \| award. Omit for all. |
| `updatedFrom` | string | no | — | Updated from. ISO datetime. Default: 7 days ago. |
| `updatedTo` | string | no | — | Updated to. ISO datetime. Default: now. |
| `maxItems` | integer (min 1, max 5000) | no | `50` | Max items. Cap on notice rows. |

**Output fields** (one dataset row per result)

| Field | Type | Notes |
|---|---|---|
| `recordType` | string | notice \| error |
| `ocid` | string or null |  |
| `releaseId` | string or null |  |
| `noticeId` | string or null |  |
| `date` | string or null |  |
| `tag` | string or null |  |
| `stage` | string or null |  |
| `title` | string or null |  |
| `buyerName` | string or null |  |
| `cpv` | string or null |  |
| `sourceUrl` | string or null |  |
| `error` | string or null |  |
| `description` | string or null |  |
| `status` | string or null |  |
| `buyerId` | string or null |  |
| `valueAmount` | number or null |  |
| `valueCurrency` | string or null |  |
| `tenderPeriodEnd` | string or null |  |
| `procurementMethod` | string or null |  |
| `packageUri` | string or null |  |

#### `publicrecords--eu-ted-notices-scraper`

TED Tenders Daily: EU procurement notices from Tenders. Use when you need EU procurement notices from TED (Tenders Electronic Daily) with buyer, CPV code, value and deadline. Store page: <https://apify.com/publicrecords/eu-ted-notices-scraper>

**Inputs**

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `keywords` | string | no | — | Keywords (full-text). Optional. Mapped to TED expert-search FT~"…" full-text. Ignored if expertQuery is set. |
| `expertQuery` | string | no | — | Expert query (advanced). Optional raw TED Expert Search expression. When set, overrides keywords / dates / noticeType / cpv / buyerCountry filters (you must include SORT BY yourself if desired). Example: buyer-country=DEU AND PD>=20260101 SORT BY publication-date DESC |
| `publicationDateFrom` | string | no | — | Publication date from (YYYY-MM-DD). Optional. Default: 14 days ago. Converted to TED PD>=YYYYMMDD. Ignored if expertQuery is set. |
| `publicationDateTo` | string | no | — | Publication date to (YYYY-MM-DD). Optional. Converted to TED PD<=YYYYMMDD. Ignored if expertQuery is set. |
| `noticeType` | string | no | — | Notice type. Optional eForms notice-type filter (e.g. cn-standard, can-standard, pin-only, pin-cfc-standard). Ignored if expertQuery is set. |
| `cpv` | string | no | — | CPV code. Optional Common Procurement Vocabulary code (e.g. 72000000). Mapped to classification-cpv. Ignored if expertQuery is set. |
| `buyerCountry` | string | no | — | Buyer country (ISO3). Optional ISO 3166-1 alpha-3 buyer country (e.g. DEU, FRA, NLD). Mapped to buyer-country. Ignored if expertQuery is set. |
| `scope` | string (one of: `ALL`, `ACTIVE`) | no | `"ALL"` | Search scope. TED scope: ALL (archive) or ACTIVE (currently open). Default ALL. |
| `maxItems` | integer (min 1, max 5000) | no | `50` | Max items. Maximum notice rows to push to the default dataset. |

**Output fields** (one dataset row per result)

| Field | Type | Notes |
|---|---|---|
| `recordType` | string |  |
| `noticeId` | string |  |
| `title` | string |  |
| `noticeType` | string |  |
| `publicationDate` | string |  |
| `buyer` | string |  |
| `buyerCountry` | string |  |
| `cpv` | string |  |
| `ojsNumber` | string or null |  |
| `htmlUrl` | string or null |  |
| `pdfUrl` | string or null |  |
| `xmlUrl` | string or null |  |
| `sourceUrl` | string |  |
| `query` | string |  |
| `retrievedAt` | string |  |
| `schemaVersion` | string |  |

#### `publicrecords--legistar-meetings-scraper`

Legistar City Council Minutes & Agendas: meetings, items. Use when you need city council or county board meetings, agenda items, votes and documents from a Legistar client (e.g. 'seattle', 'chicago'). Store page: <https://apify.com/publicrecords/legistar-meetings-scraper>

**Inputs**

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `clients` | array of string | yes | `["seattle"]` | Client slugs. Legistar client codes. Use the hostname label of {slug}.legistar.com. Working examples: seattle, milwaukee, madison, oakland, sanjose, boston, denver, columbus. |
| `dateFrom` | string | no | — | From date. Inclusive YYYY-MM-DD (EventDate / MatterIntroDate). Default: 30 days ago. |
| `dateTo` | string | no | — | To date. Inclusive YYYY-MM-DD. Default: today + 14 days. |
| `maxItems` | integer (min 1, max 5000) | no | `50` | Max items. Cap on dataset rows (events + matters combined). |
| `resource` | string (one of: `events`, `matters`, `both`) | no | `"events"` | Resource. Which Legistar collection to pull. Options: events = Meetings (events); matters = Matters (legislation); both = Both. |
| `includeEventItems` | boolean | no | `false` | Include agenda item titles. Extra HTTP call per event. Titles flattened into eventItemTitles (pipe-separated). Default off. |

**Output fields** (one dataset row per result)

| Field | Type | Notes |
|---|---|---|
| `recordType` | string | event \| matter \| error |
| `client` | string or null |  |
| `eventId` | integer or null |  |
| `bodyName` | string or null |  |
| `eventDate` | string or null |  |
| `inSiteUrl` | string or null |  |
| `matterId` | integer or null |  |
| `title` | string or null |  |
| `error` | string or null |  |
| `eventGuid` | string or null | On event rows. |
| `bodyId` | number or null | On event rows. |
| `eventTime` | string or null | On event rows. |
| `location` | string or null | On event rows. |
| `agendaStatus` | string or null | On event rows. |
| `minutesStatus` | string or null | On event rows. |
| `agendaUrl` | string or null | On event rows. |
| `minutesUrl` | string or null | On event rows. |
| `videoStatus` | string or null | On event rows. |
| `comment` | string or null | On event rows. |
| `lastModifiedUtc` | string or null | On event rows. |
| `eventItemCount` | number | On event rows. |
| `eventItemTitles` | string | On event rows. May be empty. |
| `matterGuid` | string or null | On matter rows. |
| `matterFile` | string or null | On matter rows. |
| `typeName` | string or null | On matter rows. |
| `statusName` | string or null | On matter rows. |
| `introDate` | string or null | On matter rows. |
| `agendaDate` | string or null | On matter rows. |
| `passedDate` | string or null | On matter rows. May be empty. |
| `enactmentNumber` | string or null | On matter rows. May be empty. |


## Who it's for

- GovCon and bid teams: which federal opportunities were posted today?
- Grant writers and nonprofits: which federal grants are open, and when do they close?
- Market research: who has been winning awards from an agency, and how much?
- Local news and civic watchers: what did a city council put on its agenda, and what do the minutes say?

## Why general-purpose

Any MCP client gets one place to search public-sector procurement, grants and award data from official public sources, with the source link on every row:

- **US federal contract opportunities and grants** (SAM.gov + Grants.gov, `govcon-opportunity-feed`): filter by keywords, NAICS, agency, set-aside type, place-of-performance state, notice type, posting window and deadline; optionally only items new since the last run.
- **US federal grants** (Grants.gov, `grants-gov-scraper`): filter by keyword, agency, status (forecasted, posted, closed, archived), posting window, and open and close dates.
- **US federal awards** (USAspending, `usaspending-awards-scraper`): filter by keywords, NAICS, awarding agency, recipient, award type and date range.
- **UK public tenders** (Find a Tender and Contracts Finder, `uk-find-a-tender-scraper`): filter by keywords, CPV codes, notice stage (planning, tender, award) and update window.
- **EU public tenders** (TED, `eu-ted-notices-scraper`): filter by full-text keywords, CPV code, buyer country, notice type, publication dates and active-only, or pass a raw TED expert query.
- **City council meetings and legislation** (Legistar, `legistar-meetings-scraper`): any city or county that runs on Legistar, by client slug and date range, for meetings, matters or both.

It is not tied to one client or one account: it works with any MCP client (remote Streamable HTTP, a local Node.js stdio server, or a Python package) and with any Apify account and token. The proxy code here is open source under the MIT License; the tool list is generated from the Actors' public input schemas and can be inspected with no token.

To be plain about what it is: this server is a thin, documented front end for Actors that I publish on Apify Store. The Actor source is not in this repo. My sibling servers listed under "See also" cover other domains (jobs, e-commerce, sports odds, forecasts); this one covers only public-sector tenders, grants, awards and council records. You can limit it to a subset of these Actors with `APIFY_ACTORS`.

## Example prompts

- Which new SAM.gov and Grants.gov notices mention `<keyword>` today? Include the source links.
- List open Grants.gov opportunities from `<agency>` and their deadlines.
- Which recipients won USAspending awards from `<agency>` in `<period>`, and for how much?
- Find EU TED tenders published today for IT services, with buyer and deadline.

## Cursor / Claude Code / any remote-MCP client

```json
{
  "mcpServers": {
    "gov-tenders": {
      "url": "https://mcp.apify.com/?tools=publicrecords/govcon-opportunity-feed,publicrecords/grants-gov-scraper,publicrecords/usaspending-awards-scraper,publicrecords/uk-find-a-tender-scraper,publicrecords/eu-ted-notices-scraper,publicrecords/legistar-meetings-scraper"
    }
  }
}
```

## Claude Desktop (local bridge)

```json
{
  "mcpServers": {
    "gov-tenders": {
      "command": "npx",
      "args": ["-y", "@apify/actors-mcp-server", "--actors", "publicrecords/govcon-opportunity-feed,publicrecords/grants-gov-scraper,publicrecords/usaspending-awards-scraper,publicrecords/uk-find-a-tender-scraper,publicrecords/eu-ted-notices-scraper,publicrecords/legistar-meetings-scraper"],
      "env": { "APIFY_TOKEN": "YOUR_APIFY_TOKEN" }
    }
  }
}
```

## Local stdio server (open source)

This repo also ships a small MCP server in [`src/`](src/) for clients that only run local stdio servers. It answers `initialize` and `tools/list` locally from [`tools.json`](tools.json), which is generated from each Actor's public input schema and checked for drift in CI and weekly, so any client or directory can inspect the tools with no token. `tools/call` is forwarded to the hosted Apify MCP endpoint above with **your own** `APIFY_TOKEN`; without a token a call returns a clear error instead of running anything. The token is only sent in the `Authorization` header and is never logged.

```json
{
  "mcpServers": {
    "gov-tenders": {
      "command": "npx",
      "args": ["-y", "github:GeminiGeorge22/gov-tenders-mcp"],
      "env": { "APIFY_TOKEN": "YOUR_APIFY_TOKEN" }
    }
  }
}
```

Or from a clone:

```bash
git clone https://github.com/GeminiGeorge22/gov-tenders-mcp.git
cd gov-tenders-mcp && npm install
APIFY_TOKEN=YOUR_APIFY_TOKEN node src/server.js
```

Optional env: `APIFY_ACTORS` (comma-separated subset of the Actors above) and `APIFY_MCP_URL` (full endpoint override).

Tests run against an in-memory mock server, with no network and no token: `npm test`. Requires Node.js 18+.

### Python (uvx/pip)

A matching Python version of this server lives in [`python/`](python/), built on the official `mcp` Python SDK: same tools, same `APIFY_TOKEN`, same hosted endpoint. Requires Python 3.10+.

```json
{
  "mcpServers": {
    "gov-tenders": {
      "command": "uvx",
      "args": ["gov-tenders-mcp"],
      "env": { "APIFY_TOKEN": "YOUR_APIFY_TOKEN" }
    }
  }
}
```

Or with pip: `pip install gov-tenders-mcp`, then run `gov-tenders-mcp`. Python tests: `cd python && pip install -e ".[test]" && env -u APIFY_TOKEN python -m pytest`.

## See also

- [ecommerce-search-rankings-mcp](https://github.com/GeminiGeorge22/ecommerce-search-rankings-mcp): E-commerce search results and product rankings across Amazon, eBay, Walmart, Google Shopping, AliExpress, Shopify and Etsy.
- [ecommerce-sales-mcp](https://github.com/GeminiGeorge22/ecommerce-sales-mcp): E-commerce sales signals: Amazon bestsellers and reviews, eBay sold prices, TikTok Shop and Etsy shop sales.
- [forecast-accuracy-mcp](https://github.com/GeminiGeorge22/forecast-accuracy-mcp): Forecast accuracy and track records: forecasts vs actuals, earnings beats, prediction markets, archived pages and news.
- [sports-betting-stats-mcp](https://github.com/GeminiGeorge22/sports-betting-stats-mcp): Sports betting odds and stats: sportsbook odds, player props, scores, xG, closing lines, horse racing.
- [sports-odds-mcp](https://github.com/GeminiGeorge22/sports-odds-mcp): Live odds from Kambi-powered sportsbooks (BetRivers, Unibet, Paf, Svenska Spel, ATG).
- [etsy-sales-mcp](https://github.com/GeminiGeorge22/etsy-sales-mcp): Etsy search results and Etsy shop sales counters.
- [ats-jobs-mcp](https://github.com/GeminiGeorge22/ats-jobs-mcp): New job postings from Greenhouse, Lever, Ashby and SmartRecruiters.

## Registry

Official MCP Registry name: `io.github.GeminiGeorge22/gov-tenders-mcp`

## License

MIT, for this documentation, config and the proxy server code in `src/`. Not affiliated with the sites the Actors read.
