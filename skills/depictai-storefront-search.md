---
name: Depict storefront search
description: Power an e-commerce storefront search box with Depict — as-you-type suggestions plus a full ranked, filterable results page, scoped to a market and locale.
api: openapi/depictai-storefront-openapi.json
operations:
  - get_markets_v3_markets_get
  - get_locales_v3_locales_get
  - Get_suggestions_v3_search_suggestions_get
  - Get_results_v3_search_results_post
  - Get_related_recommendations_v3_search_related_post
---

# Depict storefront search

Use the Depict Storefront API to serve search on a headless/composable store. You own the frontend; Depict returns ranked results.

## Auth
Storefront requests use an API key: send `X-API-KEY: <key>` (header), or `api_key=<key>` as a query parameter. See `authentication/depictai-authentication.yml`.

## Steps
1. Resolve the shopper's `market` and `locale`. Call `get_markets_v3_markets_get` and `get_locales_v3_locales_get` once to discover valid values for the merchant; cache them.
2. While the user types, call `Get_suggestions_v3_search_suggestions_get` with the partial `query` to render query and listing suggestions.
3. On submit, call `Get_results_v3_search_results_post` with the `query`, `market`, `locale` and any active filters/sort. Render the returned products and facets. Paginate with `limit`.
4. When the user exhausts results, call `Get_related_recommendations_v3_search_related_post` with the same request body (minus pagination) to show related products.

## Rules
- Always pass `merchant`/`tenant`, `market` and `locale` — results are scoped by them (`conventions/depictai-conventions.yml`).
- Invalid/missing fields return HTTP 422 with a `detail[]` validation array (`errors/depictai-problem-types.yml`); surface a generic error and log `detail`.
- Fire Depict Performance Client (DPC) tracking events on impressions/clicks so ranking keeps improving.
