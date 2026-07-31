---
name: Act on Opply recommendations
description: Read Opply's AI-generated supply recommendations for a brand and act on, snooze, or dismiss them.
api: openapi/opply-openapi-original.yml
operations:
  - api_v1_recommendations_list
  - api_v1_recommendations_retrieve
  - api_v1_recommendations_act_create
  - api_v1_recommendations_snooze_create
  - api_v1_recommendations_dismiss_create
---

# Act on Opply recommendations

Use the Opply API to work through the platform's AI-generated recommendations for a brand.

## Auth
- `Authorization: Token <key>` or OAuth bearer. Reads need `recommendations:read`; acting needs `recommendations:write`. See `authentication/opply-authentication.yml`.

## Steps
1. **List recommendations** — `GET /api/v1/recommendations/` (`api_v1_recommendations_list`), page-number paginated.
2. **Read one** — `GET /api/v1/recommendations/{uuid}/` (`api_v1_recommendations_retrieve`).
3. **Act on it** — `POST /api/v1/recommendations/{uuid}/act/` (`api_v1_recommendations_act_create`).
4. **Or snooze** — `POST /api/v1/recommendations/{uuid}/snooze/` (`api_v1_recommendations_snooze_create`).
5. **Or dismiss** — `POST /api/v1/recommendations/{uuid}/dismiss/` (`api_v1_recommendations_dismiss_create`).

## Error handling
- `404 "Not found"` — bad recommendation UUID or already superseded.
- `400` — the recommendation is not in a state that allows the transition (e.g. acting on a stale one). See `errors/opply-problem-types.yml`.

## Notes
- Prefer the MCP surface (`mcp/opply-mcp.yml`) with the `recommendations:*` consent scopes when driving this from an agent.
