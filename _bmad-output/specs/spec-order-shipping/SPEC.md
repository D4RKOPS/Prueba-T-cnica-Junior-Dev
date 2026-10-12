---
id: SPEC-order-shipping
companions:
  - acceptance-criteria.md
sources:
  - DOCS/review-findings.md
---

# Order Shipping Reliability

## Why

The order management flow must preserve cancelled orders and clearly report failed shipping attempts so users can trust stored order status and the table shown in the UI.

## Capabilities

- **CAP-1**
  - **intent:** Users can ship pending orders and receive a clear result when a shipping attempt cannot be completed.
  - **success:** Only a pending order transitions to shipped; missing and ineligible orders are rejected with the required status codes, failed attempts are visible, and the order table reflects current server state after every attempt. See `acceptance-criteria.md`.

## Constraints

- The server must perform a single atomic update conditioned on `status = 'pendiente'` and use the affected-row count to determine the outcome.
- SQLite connections must be closed on all execution paths.
- Keep the change small: no refactor, new dependencies, or new tests.

## Non-goals

- Do not add an empty-list message.
- Do not handle a missing database table when the seed script has not run.
- Do not fix or change seed behavior; document seed issues only.

## Success signal

A pending order becomes shipped, while a cancelled or otherwise non-pending order remains unchanged and receives a conflict response. For every attempted shipment, the user sees a clear error on failure and the reloaded table shows current server state.
