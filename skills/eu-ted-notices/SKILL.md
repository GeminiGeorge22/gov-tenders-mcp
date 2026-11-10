---
name: eu-ted-notices
description: "Returns EU procurement notices from TED (Tenders Electronic Daily), with buyer, CPV, value, deadline, and source link. Use when the user asks which EU tenders were published recently, wants TED notices by country, CPV, or notice type, or is watching European procurement. Runs the Apify Actor publicrecords/eu-ted-notices-scraper through MCP on the user's own Apify account."
license: MIT
metadata:
  author: GeminiGeorge22
  version: "1.0.0"
---

# EU TED notices

Returns EU procurement notices from TED (Tenders Electronic Daily), with buyer, CPV, value, deadline, and the source link.

The Actor is [publicrecords/eu-ted-notices-scraper](https://apify.com/publicrecords/eu-ted-notices-scraper) on Apify Store. It is also available through the [gov-tenders-mcp](https://github.com/GeminiGeorge22/gov-tenders-mcp) server.

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
      "url": "https://mcp.apify.com/?tools=publicrecords/eu-ted-notices-scraper",
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

- "Which EU tenders were published in the last 14 days in Germany for CPV 72000000?"
- "Search TED for standard contract notices in France."

Tool name: `publicrecords--eu-ted-notices-scraper`.

```json
{ "keywords": "software", "buyerCountry": "DEU", "cpv": "72000000" }
```

Useful inputs: `keywords`, `expertQuery` (raw TED Expert Search; when set it overrides the other filters), `publicationDateFrom`, `publicationDateTo`, `noticeType`, `cpv`, `buyerCountry` (ISO 3166-1 alpha-3, for example `DEU`), `scope` (`ALL` or `ACTIVE`), `maxItems`.

## Limits

- Read-only. It reads public TED data and does not submit tenders.
- If `expertQuery` is set, the other filters are ignored; include sort and constraints in that query.
- Use dates, country, CPV, and `maxItems` to keep the run scoped.
