---
name: legistar-meetings
description: "Returns Legistar city or county council meetings, agenda items, votes, and documents for a Legistar client (for example seattle or chicago), with source links. Use when the user asks what a city council decided, wants agendas or minutes, or needs legislation items from a Legistar site. Runs the Apify Actor publicrecords/legistar-meetings-scraper through MCP on the user's own Apify account."
license: MIT
metadata:
  author: GeminiGeorge22
  version: "1.0.0"
---

# Legistar meetings and legislation

Returns meetings, agenda items, votes, and documents from a city or county that publishes on Legistar, with source links.

The Actor is [publicrecords/legistar-meetings-scraper](https://apify.com/publicrecords/legistar-meetings-scraper) on Apify Store. It is also available through the [gov-tenders-mcp](https://github.com/GeminiGeorge22/gov-tenders-mcp) server.

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
      "url": "https://mcp.apify.com/?tools=publicrecords/legistar-meetings-scraper",
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

- "What did the Seattle city council take up in the last 30 days?"
- "List Chicago Legistar meetings and matters from 2026-10-01 to 2026-11-10."

Tool name: `publicrecords--legistar-meetings-scraper`.

`clients` is required. Use the hostname label of `{slug}.legistar.com` (examples that work: `seattle`, `chicago`, `milwaukee`, `madison`, `oakland`, `sanjose`, `boston`, `denver`, `columbus`).

```json
{ "clients": ["seattle"], "resource": "both" }
```

Useful inputs: `clients`, `dateFrom`, `dateTo` (YYYY-MM-DD), `maxItems`, `resource` (`events` for meetings, `matters` for legislation, or `both`), `includeEventItems`.

## Limits

- Read-only. It reads public Legistar data and does not post to the council site.
- Only jurisdictions that publish on Legistar are available. Confirm the client slug first.
- `includeEventItems` adds an extra request per event; leave it off unless the user needs item titles.
- Use date range and `maxItems` to keep the run scoped.
