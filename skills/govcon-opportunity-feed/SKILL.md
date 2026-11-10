---
name: govcon-opportunity-feed
description: "Returns US federal contract opportunities from SAM.gov and grant notices from Grants.gov in one feed, including items new since a given date and source links. Use when the user asks which federal opportunities were posted recently, wants a combined SAM.gov and Grants.gov watch list, or is capturing GovCon pursuits. Runs the Apify Actor publicrecords/govcon-opportunity-feed through MCP on the user's own Apify account."
license: MIT
metadata:
  author: GeminiGeorge22
  version: "1.0.0"
---

# Federal contract and grant opportunities (GovCon)

Returns US federal contract opportunities (SAM.gov) and grant notices (Grants.gov) in one feed, with optional new-since-last-run selection and source links.

The Actor is [publicrecords/govcon-opportunity-feed](https://apify.com/publicrecords/govcon-opportunity-feed) on Apify Store. It is also available through the [gov-tenders-mcp](https://github.com/GeminiGeorge22/gov-tenders-mcp) server.

## Authentication

Runs on **the user's own Apify account**.

- Use an API token the user created in their own Apify Console, or sign in with Apify OAuth when the MCP client prompts.
- Put that token only in the client's MCP config or environment as `APIFY_TOKEN` (or as `Authorization: Bearer <token>` on the hosted endpoint).
- Never use a shared, publisher, or third-party token. Never paste a token into chat.
- `tools/list` works without a token. `tools/call` needs the user's token and runs on that user's Apify account.

The default SAM.gov path uses a published bulk feed and does not need a SAM.gov API key. If the user wants API enrich or monitor checks that require a SAM.gov key, they must supply **their own** personal public API key from sam.gov in the `samApiKey` input. Never use a publisher or shared SAM.gov key.

## Connect the MCP server

```json
{
  "mcpServers": {
    "gov-tenders": {
      "url": "https://mcp.apify.com/?tools=publicrecords/govcon-opportunity-feed",
      "headers": { "Authorization": "Bearer <YOUR_APIFY_TOKEN>" }
    }
  }
}
```

To load all government Actors in this repo, use the combined tools list in the [gov-tenders-mcp README](https://github.com/GeminiGeorge22/gov-tenders-mcp). Local stdio:

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

- "Which federal contract opportunities were posted in the last 14 days for NAICS 541512?"
- "Show new SAM.gov and Grants.gov notices since 2026-11-01 for the Department of Defense."

Tool name: `publicrecords--govcon-opportunity-feed`.

```json
{ "sources": ["sam", "grants"], "naics": ["541512"], "postedWithinDays": 14 }
```

Useful inputs: `sources` (`sam`, `grants`), `naics`, `agencies`, `keywords`, `setAside`, `states`, `noticeTypes`, `postedWithinDays`, `deadlineAfter`, `since`, `includeDescriptions`, `includeContacts`, `maxItems`.

## Limits

- Read-only. It reads public opportunity data and does not change anything on SAM.gov or Grants.gov.
- Filter to the user's agencies, NAICS, dates, and `maxItems` so the run stays scoped.
