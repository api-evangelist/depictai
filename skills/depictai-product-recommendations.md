---
name: Depict product recommendations
description: Render personalized product recommendation surfaces (e.g. "You may also like", "Frequently bought together") on a storefront using Depict's recommendation displays.
api: openapi/depictai-storefront-openapi.json
operations:
  - get_markets_v3_markets_get
  - get_locales_v3_locales_get
  - Recommendations_v3_recommend_products_post
---

# Depict product recommendations

Use the Depict Storefront API to fetch recommendation displays for a product page, cart or home page.

## Auth
Send the Storefront API key as `X-API-KEY: <key>` (header) or `api_key=<key>` (query). See `authentication/depictai-authentication.yml`.

## Steps
1. Resolve the shopper's `market` and `locale` (`get_markets_v3_markets_get`, `get_locales_v3_locales_get`); cache per merchant.
2. Call `Recommendations_v3_recommend_products_post` with the seed `product_ids`, the `market`, the `locale`, and the shopper `session_id`/`user_id` when known.
3. The response is an object with an array of recommendation **displays**; render each display's products in its own carousel/grid, preserving Depict's order.

## Rules
- Pass session/user identifiers when available so recommendations are personalized; they degrade gracefully to non-personalized when absent (no historical data required).
- Scope every call by `merchant`/`tenant`, `market` and `locale` (`conventions/depictai-conventions.yml`).
- Validation failures return HTTP 422 with `detail[]` (`errors/depictai-problem-types.yml`).
- Track impressions/clicks with the Depict Performance Client so the models keep learning.
