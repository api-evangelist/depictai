---
name: Depict product listing pages
description: Build AI-merchandised category / collection listing pages on a headless storefront using Depict's v3 listing hierarchy and per-listing product queries.
api: openapi/depictai-storefront-openapi.json
operations:
  - Get_Listings_v3_listings_get
  - Get_Listing_v3_listings__listing_id__get
  - Get_Products_in_Listing_v3_listings__listing_id__products_post
---

# Depict product listing pages (PLPs)

Use the Depict Storefront API to render merchandised category/collection pages.

## Auth
Send the Storefront API key as `X-API-KEY: <key>` (header) or `api_key=<key>` (query). See `authentication/depictai-authentication.yml`.

## Steps
1. Call `Get_Listings_v3_listings_get` to fetch the full listing hierarchy for navigation menus and PLP routing (optionally filtered by listing type).
2. For a given page, call `Get_Listing_v3_listings__listing_id__get` (by Depict `listing_id`, or use the external-ID variant with the merchant's own ID) to get the listing plus its ancestors, siblings and children.
3. Call `Get_Products_in_Listing_v3_listings__listing_id__products_post` with the shopper's `market`/`locale` and any active filters to get the merchandised, ranked products for the page. Paginate with `limit`.

## Rules
- Prefer Depict IDs; use the `external_id` operations when you only hold the merchant's (Shopify/Centra/WooCommerce) IDs.
- Scope by `merchant`/`tenant`, `market` and `locale` (`conventions/depictai-conventions.yml`).
- HTTP 422 signals a validation error with `detail[]` (`errors/depictai-problem-types.yml`).
- Emit DPC tracking for PLP views and product-card clicks.
