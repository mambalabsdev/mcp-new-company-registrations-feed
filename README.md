# New Company Registrations Feed MCP Server

[![LightNow capabilities](https://lightnow.ai/badge/com.mambabuilt/mcp-new-company-registrations-feed)](https://lightnow.ai/servers/com.mambabuilt/mcp-new-company-registrations-feed) [![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-new-company-registrations-feed/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-new-company-registrations-feed) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-new-company-registrations-feed%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-new-company-registrations-feed&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-new-company-registrations-feed)](https://www.npmjs.com/package/@mambalabsdev/mcp-new-company-registrations-feed) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-new-company-registrations-feed)](https://www.npmjs.com/package/@mambalabsdev/mcp-new-company-registrations-feed) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-new-company-registrations-feed)](https://github.com/mambalabsdev/mcp-new-company-registrations-feed/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-new-company-registrations-feed)

An MCP server for the Mamba Labs New Company Registrations Feed actor on Apify. Give it a registration window, industry codes, activity words, or places. Each run reads four official business registers and returns one row per newly registered company.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Your keys](#your-keys)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

The actor reads UK Companies House (on your own free Companies House key), Norway Brreg, Finland PRH, and France BODACC company creation notices, keeps the companies registered in your window that match your filters, and returns one flat row per company: register number, name, legal form, registration day, industry codes with their description, activity text, registered address, share capital, and the website and domain when the register lists one. Sole traders are never returned, filtered entries are never billed, and when nothing matches one free row says why.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-new-company-registrations-feed": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-new-company-registrations-feed"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `find_new_company_registrations` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "List IT companies registered in the UK and Norway this week."
- "Which new French companies registered in Paris or Lyon this week do software or IT consulting?"
- "Find new restaurants and cafes registered in Finland and Norway in the last 30 days."

## Tool

`find_new_company_registrations`. One tool. Example arguments:

```json
{
  "industry_codes": [
    "62"
  ],
  "registered_within_days": "7"
}
```

## Inputs

Every buyer facing input of the actor, unchanged. Omitted inputs take the actor's own defaults. Numbers and booleans are sent as strings.

| Input | Type | Description |
|---|---|---|
| `registers` | array | Registers to read. Default all four. UK Companies House is read only when the server has COMPANIES_HOUSE_API_KEY set. |
| `registered_within_days` | string | Registration window in days, 1 to 90, today included. Default 7. |
| `industry_codes` | array | Industry code prefixes such as 62, 70.2, or 56. France BODACC has no codes and is skipped when this is set. |
| `activity_keywords` | array | Words in the activity text the register publishes, in its own language, for example logiciel (France) or programvare (Norway). |
| `name_keywords` | array | Words in the company name. |
| `locations` | array | City or region names, or short postal code prefixes such as EC1 or 75. |
| `active_only` | string | Drop entries already marked dissolved, bankrupt, or in liquidation. Default true. |
| `max_companies` | string | Company rows per run, newest first, up to 5000. Each company row is billed. Default 200. |
| `only_new` | string | Skip companies an earlier run with the same state_key returned. Default false. |
| `state_key` | string | Names the seen list for only_new. |
| `max_charge_usd` | string | Optional. The run stops before any row that would take its charges over this amount. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `company-row`, per company row | $0.0035 | $0.003325 | $0.00315 | $0.002975 |
| `apify-actor-start`, per run start | $0.002 | $0.002 | $0.002 | $0.002 |

The no results row and companies skipped by `only_new` are free.

## Your keys

Norway, Finland, and France need no key. For UK Companies House, set your own free Companies House REST API key as the `COMPANIES_HOUSE_API_KEY` environment variable of this server; it is sent with each run as the actor's `companies_house_api_key` input, which the actor marks secret and never stores. Without it the UK register is skipped and the run says so. Mamba Labs pays the platform and proxy costs of the run.

## Full actor documentation

[apify.com/mambalabs/new-company-registrations-feed](https://apify.com/mambalabs/new-company-registrations-feed)

## License

MIT. See [LICENSE](LICENSE).
