---
name: Source an ingredient and review quotes
description: Raise an ingredient sourcing inquiry on Opply and review, decline, or progress the supplier quotes that come back.
api: openapi/opply-openapi-original.yml
operations:
  - api_v1_inquiries_ingredients_list
  - api_v1_inquiries_ingredients_create
  - api_v1_inquiries_quotes_brand_list
  - api_v1_inquiries_quotes_brand_retrieve
  - api_v1_inquiries_quotes_brand_decline_create
---

# Source an ingredient and review quotes

Use the Opply API to source an ingredient and evaluate the supplier quotes returned against the inquiry.

## Auth
- `Authorization: Token <key>` or OAuth bearer. Catalog/sourcing reads need `catalog:read`; writes need `catalog:write`. See `authentication/opply-authentication.yml`.

## Steps
1. **List existing ingredient inquiries** — `GET /api/v1/inquiries/ingredients/` (`api_v1_inquiries_ingredients_list`).
2. **Create a sourcing inquiry** — `POST /api/v1/inquiries/ingredients/` (`api_v1_inquiries_ingredients_create`) describing the ingredient/spec needed.
3. **List quotes for the inquiry** — `GET /api/v1/inquiries/{inquiry_uuid}/quotes/brand/` (`api_v1_inquiries_quotes_brand_list`).
4. **Inspect a quote** — `GET /api/v1/inquiries/{inquiry_uuid}/quotes/brand/{uuid}/` (`api_v1_inquiries_quotes_brand_retrieve`).
5. **Decline unwanted quotes** — `POST /api/v1/inquiries/{inquiry_uuid}/quotes/brand/{uuid}/decline/` (`api_v1_inquiries_quotes_brand_decline_create`).

## Error handling
- `400 "Validation error"` — field-keyed validation object; inspect each field's messages.
- `404 "Not found"` — bad inquiry or quote UUID.
- `403` — the token's company has no access to this inquiry. See `errors/opply-problem-types.yml`.

## Notes
- Quotes and inquiries are UUID-addressed; list endpoints are page-number paginated (`?page=&page_size=`).
