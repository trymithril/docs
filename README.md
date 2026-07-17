# Mithril docs

The public documentation for [Mithril](https://trymithril.com) — one REST API
for trading Kalshi and Polymarket. Built with [Mintlify](https://mintlify.com).

## Structure

- `index.mdx` — introduction / positioning
- `quickstart.mdx` — API key → first risk-checked order
- `authentication.mdx` — bearer keys
- `concepts/` — conventions, idempotency, errors, rate limits
- `guides/` — one page per stack layer (accounts, market data, guardrails,
  orders, smart execution, risk)
- `api-reference/` — `overview.mdx` plus `openapi.json` (a committed, native
  interactive reference; regenerate it from the gateway, see below)
- `docs.json` — site config (navigation, branding, colors)

The prose here is the source of truth for *how to talk to the gateway*. The
machine-generated endpoint list (paths, auth, rate-limit class) is served from
the gateway itself and linked from the API reference tab.

## Local preview

Requires an LTS Node version (the Mintlify CLI does not support Node 25+).

```bash
npm i -g mint
mint dev          # http://localhost:3000
mint broken-links # validate internal links
```

## Publishing

The Mintlify GitHub app auto-deploys on push to the default branch.

## Regenerating the API reference

`api-reference/openapi.json` is generated from the gateway's route table. When
endpoints change in `gateway/src/api/routes.go`, regenerate it:

```bash
cd ../gateway/src
OPENAPI_OUT=$(pwd)/../../docs/api-reference/openapi.json \
  go test -run TestGenerateOpenAPISpec ./api/
```

Mintlify renders the committed spec as the native **Endpoints** reference, so it
never links out to a separate Redoc page.

## Keeping it accurate

Document only what the API actually does today — market data is REST/polling (no
websockets yet), and conditional TP/SL orders are rejected at submission until
the trigger engine ships.
