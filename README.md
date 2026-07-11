# hydrafetch-docs

The public documentation for the Hydrafetch API, built with [Mintlify](https://mintlify.com). This is a standalone repository — the API code lives in the separate Hydrafetch monorepo. Keeping docs in their own repo means Mintlify's GitHub app only ever sees the docs, never the backend.

## The API reference is a committed snapshot

`docs.json` points its API Reference tab at the `openapi.json` file committed in this repo. That keeps builds reproducible, offline, and independent of the API — Mintlify fetches a `openapi` URL at build time, so referencing a live URL would make every docs build (and preview) fail whenever the API is down or not yet deployed.

The spec is generated from the backend, so **do not hand-edit it**. When the API surface changes, refresh the snapshot: run `pnpm --filter @hydrafetch/backend openapi:generate` in the monorepo and copy its `apps/backend/openapi.json` over this file. That's the only sync, and it's rare.

(The API also serves the same spec live at `GET /openapi.json` — handy for the dashboard playground and for consumers who want the always-current spec — but the docs intentionally use the committed snapshot rather than depend on it.)

## Preview locally

```sh
npm i -g mint      # one-time
mint dev           # serves http://localhost:3000
```

## Structure

- `docs.json` — site config: branding, navigation, the API Reference tab (served-URL).
- `openapi.json` — local snapshot for offline preview (do not hand-edit; it comes from the backend).
- `introduction.mdx`, `quickstart.mdx`, `authentication.mdx` — the get-started guides.
- `concepts/` — formats, credits, caching, jobs & webhooks, errors.
- `endpoints/` — one narrative guide per endpoint.
- `logo/`, `favicon.svg` — **placeholder** brand assets; replace with the real logo.

## Deploy

Connect this repository to Mintlify's GitHub app; it auto-deploys on push. Everything here is portable (MDX + a referenced OpenAPI spec), so the site can move to another renderer without rewriting content.

Keep all copy **mechanism-free**: describe outcomes, never the fetch/render pipeline, engines, proxies, or models.
