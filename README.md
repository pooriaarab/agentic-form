# agentic-form

Accept one form post. Validate it. Store a lead.

## Install

```sh
npm install agentic-form
```

## Quick start

Dry run is the default. It stores nothing.

```sh
npx agentic-form --file lead.json
```

## Three uses

### CLI

```sh
agentic-form --file lead.json --live
```

### Library

```ts
import { storeLead } from "agentic-form";
await storeLead({ name: "Ada", email: "ada@example.com" });
```

### MCP

```sh
npx agentic-form-mcp
```

Use `store_lead`. Pass `dryRun: false` only when ready. Local storage is the default. Use `--sink webhook` with `AGENTIC_FORM_WEBHOOK_URL`, or `--sink agentic-crm` with `AGENTIC_CRM_URL` and `AGENTIC_CRM_API_KEY`.
