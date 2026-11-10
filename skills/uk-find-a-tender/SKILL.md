---
name: uk-find-a-tender
description: "Returns UK public-sector tenders from Find a Tender and Contracts Finder, with buyer, value, deadline, and source link. Use when the user asks which UK government tenders were published recently, wants notices by keyword or CPV, or is watching UK procurement. Runs the Apify Actor publicrecords/uk-find-a-tender-scraper through MCP on the user's own Apify account."
license: MIT
metadata:
  author: GeminiGeorge22
  version: "1.0.0"
---

# UK Find a Tender and Contracts Finder

Returns UK public-sector tender notices from Find a Tender and Contracts Finder, with buyer, value, deadline, and the source link.

The Actor is [publicrecords/uk-find-a-tender-scraper](https://apify.com/publicrecords/uk-find-a-tender-scraper) on Apify Store. It is also available through the [gov-tenders-mcp](https://github.com/GeminiGeorge22/gov-tenders-mcp) server.

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
      "url": "https://mcp.apify.com/?tools=publicrecords/uk-find-a-tender-scraper",
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

- "Which UK government tenders were updated in the last 7 days for software?"
- "Show Find a Tender and Contracts Finder notices in the tender stage with CPV 72000000."

Tool name: `publicrecords--uk-find-a-tender-scraper`.

```json
{ "keywords": ["software"], "noticeType": ["tender"] }
```

Useful inputs: `keywords`, `cpv`, `noticeType` (`planning`, `tender`, `award`), `updatedFrom`, `updatedTo`, `maxItems`.

## Limits

- Read-only. It reads public UK tender data and does not submit bids.
- Use date, keyword, CPV, and `maxItems` filters to keep the run scoped.
