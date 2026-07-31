---
name: Place and track a brand order
description: Create an order for a food & beverage brand on Opply and follow it through to confirmed delivery, including raising a dispute if something is wrong.
api: openapi/opply-openapi-original.yml
operations:
  - api_v1_orders_brand_list
  - api_v1_orders_brand_create
  - api_v1_orders_brand_retrieve
  - api_v1_orders_brand_confirm_delivered_create
  - api_v1_orders_brand_disputes_create
---

# Place and track a brand order

Use the Opply API to place an order on behalf of a food & beverage brand and follow it to delivery.

## Auth
- Send `Authorization: Token <key>` (or an OAuth 2.1 bearer token). See `authentication/opply-authentication.yml`.
- Order write access needs the `orders:write` scope; reads need `orders:read`.

## Steps
1. **Review existing orders** — `GET /api/v1/orders/brand/` (`api_v1_orders_brand_list`). Supports `?page=&page_size=&ordering=&search=` (see `conventions/opply-conventions.yml`).
2. **Create the order** — `POST /api/v1/orders/brand/` (`api_v1_orders_brand_create`) with the order body.
3. **Poll the order** — `GET /api/v1/orders/brand/{order_uuid}/` (`api_v1_orders_brand_retrieve`) to read lifecycle/delivery state.
4. **Confirm delivery** — once received, `POST /api/v1/orders/brand/{order_uuid}/confirm-delivered/` (`api_v1_orders_brand_confirm_delivered_create`).
5. **Raise a dispute (if needed)** — `POST /api/v1/orders/brand/{order_uuid}/disputes/` (`api_v1_orders_brand_disputes_create`).

## Error handling
- `400 "Cannot transition order to this state"` — the order is not in a state that allows the action; re-read the order first.
- `403 "No access to this buyer"` — the token's company cannot act for that buyer.
- `404 "Buyer not found"` / `"Not found"` — bad UUID.
- `429` — rate limited; back off and retry. See `errors/opply-problem-types.yml`.

## Notes
- No client-side idempotency key is supported; do not blind-retry `POST` on timeout — re-read the order and reconcile first.
