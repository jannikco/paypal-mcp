# PayPal MCP server

Inspect sandbox invoices and prepare a draft invoice for review without sending it.

A direct-provider MCP server with useful local execution. No PipMCP account is required for local use. Provider API access and any provider charges are your responsibility.

## Quick start

Requires Node.js22+. Clone this repository, then run:

```sh
npm ci --ignore-scripts
npm test
npm start
```

The default command inspects the tools offline. Configure your own credentials in your environment:

- `PAYPAL_ACCESS_TOKEN`

For local OAuth, use node local-auth/cli.mjs login ./connector.json with your own provider app; see AUTH.md. Your tokens remain in the native OS credential store.

Credential help: https://developer.paypal.com/api/rest/

## Execute a real read

Replace example record IDs with records you are authorized to access.

```sh
npm start -- --call paypal_list_invoices '{}'
```

## Use with an MCP client

Launch `node /absolute/path/paypal-mcp/server.mjs` using stdio transport; pass credentials through the client environment. Stdout is reserved for JSON-RPC.

## Tools

| Tool | Effect | Purpose |
| --- | --- | --- |
| paypal_list_invoices | Read | paypal list invoices |
| paypal_get_invoice | Read | paypal get invoice |
| paypal_create_draft_invoice | Explicit approval required | Create a draft invoice; does not send it or collect payment. |

## Concrete workflow

Inspect sandbox invoices and prepare a draft invoice for review without sending it.

1. paypal_list_invoices
2. paypal_get_invoice
3. paypal_create_draft_invoice

## Optional writes

Writes are disabled by default. Set CONNECTOR_ALLOW_WRITES=true and supply `_approval: {approved:true,idempotencyKey:"unique-request-id"}` on the specific call. Do not reuse a key for a different request. Uncertain outcomes are never automatically replayed; inspect the provider before another attempt.

## Capability evidence

Local: **Tested — HTTP fixtures and real stdio tools/call**. Managed: **Documented**. Provider authentication and live operations have not been established by these fixture tests. Evidence applies per operation and execution mode; setup discovery does not imply provider execution.

## Managed production

[Activate with PipMCP](https://pipmcp.com/github/start?app=paypal&integration=paypal-mcp&utm_source=github&utm_campaign=github-paypal-mcp&utm_content=readme) when you want the hosted service.  Local credentials are not uploaded by this code. Review provider permissions in the managed onboarding. [Directory and related projects](https://jannikco.github.io/pipmcp-connectors/integrations/paypal/).

## Provenance and maintenance

- https://raw.githubusercontent.com/paypal/paypal-rest-api-specifications/main/openapi/invoicing_v2.json

Maintainer:jannikco. MIT license for this code. See connector.json for selected operations and integration.json for evidence. Runtime dependencies retain their own licenses. Report sensitive issues through https://pipmcp.com/contact; do not post credentials or customer data in public issues.
