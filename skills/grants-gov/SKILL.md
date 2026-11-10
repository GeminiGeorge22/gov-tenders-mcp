---
name: grants-gov
description: "Searches Grants.gov for US federal grant opportunities and returns opportunity title, agency, deadline, eligibility, and the source link. Use when the user asks which federal grants are open, wants grants by agency or keyword, or needs deadlines and eligibility from Grants.gov. Runs the Apify Actor publicrecords/grants-gov-scraper through MCP on the user's own Apify account."
license: MIT
metadata:
  author: GeminiGeorge22
  version: "1.0.0"
---

# Grants.gov search

Searches Grants.gov and returns federal grant opportunities with agency, deadline, eligibility, and the source link.

The Actor is [publicrecords/grants-gov-scraper](https://apify.com/publicrecords/grants-gov-scraper) on Apify Store. It is also available through the [gov-tenders-mcp](https://github.com/GeminiGeorge22/gov-tenders-mcp) server. For a combined SAM.gov plus Grants.gov feed, use the **govcon-opportunity-feed** skill in this repo.

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
      "url": "https://mcp.apify.com/?tools=publicrecords/grants-gov-scraper",
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

- "Which federal grants are open right now for cybersecurity at NSF?"
- "List posted Grants.gov opportunities from HHS with close dates in the next 60 days."

Tool name: `publicrecords--grants-gov-scraper`.

```json
{ "keyword": "cybersecurity", "agency": "NSF", "status": "posted" }
```

Useful inputs: `keyword`, `agency`, `status` (`forecasted`, `posted`, `closed`, `archived`, or pipe-joined), `postedWithinDays`, `openDateFrom`, `openDateTo`, `closeDateFrom`, `closeDateTo`, `maxItems`, `fetchDetails`.

## Limits

- Read-only. It reads public Grants.gov data and does not submit applications.
- Use `status` and date filters plus `maxItems` to keep the run scoped.
