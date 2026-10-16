# agentic-form

agentic-form validates form posts and returns leads for webhooks and Agentic CRM. Use the CLI or MCP server to preview a lead, then send it live when ready.

## Install

```sh
npm install agentic-form
```

## Quick start

Dry run is the default. It stores nothing and does not use the network.

```sh
npx agentic-form --file lead.json
```

## Live vendors

| Brand | Environment key | Live command |
| --- | --- | --- |
| Webhook | `AGENTIC_FORM_WEBHOOK_URL` | `agentic-form --file lead.json --live --sink webhook` |
| Agentic CRM | `AGENTIC_CRM_URL`, `AGENTIC_CRM_API_KEY` | `agentic-form --file lead.json --live --sink agentic-crm` |

## CLI

Use `--live` to store locally or call a configured live sink.

```sh
agentic-form --file lead.json --live
```

## Library

```ts
import { storeLead } from "agentic-form";

const result = await storeLead({ name: "Ada", email: "ada@example.com" });
```

## MCP

```sh
npx agentic-form-mcp
```

Use `validate_lead` to validate a post. Use `store_lead` to return a lead. `dryRun` defaults to `true`.
