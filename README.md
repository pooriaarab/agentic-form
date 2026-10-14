# agentic-form

[![npm version](https://img.shields.io/npm/v/agentic-form)](https://www.npmjs.com/package/agentic-form)
[![npm downloads](https://img.shields.io/npm/dm/agentic-form)](https://www.npmjs.com/package/agentic-form)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue)](https://opensource.org/license/mit)

agentic-form is an open-source form lead validator for Node.js developers and agents that writes local records or posts to webhooks and Agentic CRM.

## Install

```sh
npm install agentic-form
```

Requires Node.js 18 or later. Includes TypeScript declarations and three runtime dependencies.

## Store your first lead

```sh
printf '{"name":"Ada","email":"ADA@example.com"}' > lead.json
npx agentic-form --file lead.json
npx agentic-form --file lead.json --live --store ./leads.ndjson
cat leads.ndjson
```

Preview prints `dryRun: true` with `ada@example.com` and writes nothing.
Live mode appends a record. The final command shows `id`, `storedAt`, and the normalized `lead`.
No credentials are needed for local storage.

## Features and coverage

- **Validation:** Require a nonempty name and email syntax; accept an optional string message.
- **Normalization:** Trim names and lowercase emails; retain additional fields.
- **Local storage:** Append newline-delimited JSON (NDJSON) records.
- **HTTP sinks:** POST a lead to a webhook or a wrapped lead to Agentic CRM.

| Sink | Status | Required CLI environment |
| --- | --- | --- |
| Local filesystem | Implemented; local test | None |
| Webhook | Implemented; mocked HTTP test | `AGENTIC_FORM_WEBHOOK_URL` |
| Agentic CRM | Implemented HTTP path; mocked test; live endpoint unverified | `AGENTIC_CRM_URL`, `AGENTIC_CRM_API_KEY` |

Three sinks exist. Remote tests use injected responses; no live endpoint or credentials were checked.
Export these variables in your shell. The package does not load `.env` files.

```sh
npx agentic-form --file lead.json --live --sink webhook
npx agentic-form --file lead.json --live --sink agentic-crm
```

Remote success returns `dryRun: false`, `id`, and `storedAt`; confirm receipt at your endpoint.
Agentic CRM receives `{ lead }` with a bearer token. Webhooks receive the lead object directly.

## Library and agent tools

```js
import { storeLead } from "agentic-form";
const result = await storeLead({ name: "Ada", email: "ADA@example.com" });
console.log(result.dryRun, result.lead.email); // true ada@example.com
```

Pass `dryRun: false` to write or send. Library options include `sink`, `storePath`, `webhookUrl`, `crmUrl`, and `apiKey`.
The CLI has no subcommands; flags are `--file`, `--live`, `--sink`, and `--store`.
Run `npx --package agentic-form agentic-form-mcp` for the Model Context Protocol (MCP) stdio server.
Its two tools are `validate_lead` and `store_lead`. MCP storage supports only the default local sink.
Set MCP `dryRun: false` to append to `.agentic-form/leads.ndjson`.

## Limits and status

Email validation checks syntax, not mailbox ownership. Extra fields are retained without schema validation.
No HTTP form server, spam protection, consent enforcement, or duplicate suppression is included.
Remote requests have no built-in retry or explicit timeout. Receipt IDs are generated locally.
Successful HTTP status does not prove downstream processing. Local file writes are synchronous.
Version 0.1.1 has an API that may change. The MCP server identifies itself as 0.1.0.
No managed form service is included.

## Links and license

[Package and releases](https://www.npmjs.com/package/agentic-form) · [MIT license](https://opensource.org/license/mit)
