# Code Review Findings

Severity reflects the impact of the behavior described in the project README. Findings are based on `app.py`, `seed.py`, and `templates/index.html`, with the README and seed data used as context.

## High

### Cancelled orders can be shipped

**Evidence:** In `app.py`, the ship endpoint rejects an order only when its status is `enviado`. It then updates any other status to `enviado`, including `cancelado`. The seeded data includes cancelled orders, and the README says cancelled orders must remain cancelled.

## Medium

### Update failures are not shown to the user

**Evidence:** In `templates/index.html`, `ship()` catches failed requests and only re-enables the button. The API error response is discarded, so the user gets no visible explanation. The README identifies this as an issue.

## Low

### An empty order list renders as a blank table

**Evidence:** In `templates/index.html`, `loadOrders()` clears the table body and adds rows only while iterating over orders. If the API returns `[]`, the table is empty and the count says `0 pedidos`, without an explicit empty-state message.

### Database initialization is required before the app can serve orders

**Evidence:** In `app.py`, `GET /api/orders` reads the `orders` table, but the table is created by running `seed.py`. If the database file or table is missing, the endpoint returns a generic 500 response. The README documents running the seed script first, so this occurs if that setup step is skipped or the local database is removed.

## Scope note

The analyst skill's activation scripts could not be run because `uv` is unavailable in the environment. The skill instructions were reviewed directly. No code changes were made.
