---
name: usaspending-awards
description: "Pulls USAspending award records for US federal contracts and grants: who won, for how much, by agency, recipient, NAICS, and period, with award IDs and source links. Use when the user asks who won federal contracts or grants, wants award history for an agency or recipient, or is researching incumbents. Runs the Apify Actor publicrecords/usaspending-awards-scraper through MCP on the user's own Apify account."
license: MIT
metadata:
  author: GeminiGeorge22
  version: "1.0.0"
---

# USAspending awards

Pulls award records from USAspending for federal contracts and grants, filtered by agency, recipient, NAICS, and period, with award IDs and source links.

The Actor is [publicrecords/usaspending-awards-scraper](https://apify.com/publicrecords/usaspending-awards-scraper) on Apify Store. It is also available through the [gov-tenders-mcp](https://github.com/GeminiGeorge22/gov-tenders-mcp) server.

## Authentication

Runs on **the user's own Apify account**.

- Use an API token the user created in their own Apify Console, or sign in with Apify OAuth when the MCP client prompts.
- Put that token only in the client's MCP config or environment as `APIFY_TOKEN` (or as `Authorization: Bearer <token>` on the hosted endpoint).
- Never use a shared, publisher, or third-party token. Never paste a token into chat.
- `tools/list` works without a token. `tools/call` needs the user's token and runs on that user's Apify account.

## Connect the MCP server

```json
{
  "mcpServers": {
    "gov-tenders": {
      "url": "https://mcp.apify.com/?tools=publicrecords/usaspending-awards-scraper",
      "headers": { "Authorization": "Bearer <YOUR_APIFY_TOKEN>" }
    }
  }
}
```

Local stdio (all government Actors in this repo):

```json
{
  "mcpServers": {
    "gov-tenders": {
      "command": "npx",
      "args": ["-y", "github:GeminiGeorge22/gov-tenders-mcp"],
      "env": { "APIFY_TOKEN": "<YOUR_APIFY_TOKEN>" }
    }
  }
}
```

Replace `<YOUR_APIFY_TOKEN>` with the user's own token.

## Usage

Example prompts:

- "Who won Department of Defense contracts under NAICS 541512 in the last 30 days?"
- "List recent USAspending awards to Boeing and include source links."

Tool name: `publicrecords--usaspending-awards-scraper`.

```json
{ "agency": "Department of Defense", "naics": ["541512"], "startDate": "2026-10-01", "endDate": "2026-11-10" }
```

Useful inputs: `keywords`, `naics`, `agency`, `recipient`, `awardTypes`, `startDate`, `endDate`, `subawards`, `fetchDetails`, `maxItems`. Prefer agency, recipient, or date filters over a broad empty search.

## Limits

- Read-only. It reads public USAspending data and does not change awards.
- Default `awardTypes` are contract and IDV codes. Avoid assistance codes that may list individuals unless the user clearly asks for those award types.
- Use `maxItems` and date windows to keep the run scoped.
