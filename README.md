# Gov Tenders & Grants MCP: SAM.gov, Grants.gov, USAspending, UK, EU TED

**Public-sector opportunity data as MCP tools**: US federal contract opportunities (SAM.gov), federal grants (Grants.gov), federal awards (USAspending), UK tenders (Find a Tender, Contracts Finder), EU tenders (TED) and city council agendas and votes (Legistar). Every row carries its official source link.

Hosted remote MCP server (Streamable HTTP) on [Apify's MCP gateway](https://mcp.apify.com). **Manifest-only repo: no Actor source.**

**Disclosure:** I maintain these Actors on Apify Store (publisher `publicrecords`).

## MCP endpoint

```
https://mcp.apify.com/?tools=publicrecords/govcon-opportunity-feed,publicrecords/grants-gov-scraper,publicrecords/usaspending-awards-scraper,publicrecords/uk-find-a-tender-scraper,publicrecords/eu-ted-notices-scraper,publicrecords/legistar-meetings-scraper
```

Auth: sign in with Apify (OAuth) when your client prompts, or send an Apify API token as a Bearer token. Runs are billed per result on your own Apify account; see each Store page for current pricing.

## Tools

| Actor | Store page | MCP tool | What it returns |
|---|---|---|---|
| **Federal Contract Opportunities** | [publicrecords/govcon-opportunity-feed](https://apify.com/publicrecords/govcon-opportunity-feed) | `publicrecords--govcon-opportunity-feed` | SAM.gov and Grants.gov notices in one feed, with new-since-last-run deltas and source links. |
| **Grants.gov Search** | [publicrecords/grants-gov-scraper](https://apify.com/publicrecords/grants-gov-scraper) | `publicrecords--grants-gov-scraper` | Open federal grants: opportunity, agency, deadline, eligibility and source link. |
| **USAspending Awards** | [publicrecords/usaspending-awards-scraper](https://apify.com/publicrecords/usaspending-awards-scraper) | `publicrecords--usaspending-awards-scraper` | Federal contract and grant awards by agency, recipient, NAICS and period, with award IDs and source links. |
| **UK Find a Tender & Contracts Finder** | [publicrecords/uk-find-a-tender-scraper](https://apify.com/publicrecords/uk-find-a-tender-scraper) | `publicrecords--uk-find-a-tender-scraper` | UK government tender notices with buyer, value, deadline and source link. |
| **EU TED Tenders** | [publicrecords/eu-ted-notices-scraper](https://apify.com/publicrecords/eu-ted-notices-scraper) | `publicrecords--eu-ted-notices-scraper` | EU procurement notices from Tenders Electronic Daily with buyer, CPV, value, deadline and source link. |
| **Legistar Council Meetings** | [publicrecords/legistar-meetings-scraper](https://apify.com/publicrecords/legistar-meetings-scraper) | `publicrecords--legistar-meetings-scraper` | City council meetings, agenda items, votes and documents for any city on Legistar, with source link. |

The Apify gateway also exposes helper tools to read run results (`get-actor-run`, `get-dataset-items`).

## Who it's for

- GovCon and bid teams: which federal opportunities were posted today?
- Grant writers and nonprofits: which federal grants are open, and when do they close?
- Market research: who has been winning awards from an agency, and how much?
- Local news and civic watchers: what did a city council put on its agenda and vote on?

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

## Registry

Official MCP Registry name: `io.github.GeminiGeorge22/gov-tenders-mcp`

## License

MIT, for this documentation and config only. Not affiliated with the sites the Actors read.
