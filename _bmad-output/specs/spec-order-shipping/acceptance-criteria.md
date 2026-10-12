# Acceptance Criteria

1. **Ship a pending order:** `POST /api/orders/<id>/ship` changes `pendiente` to `enviado` and returns the updated order.
2. **Reject a missing order:** The endpoint returns HTTP 404 when the ID does not exist.
3. **Reject non-pending orders:** The endpoint returns HTTP 409 for `enviado`, `cancelado`, or any other non-pending status; the stored status remains unchanged.
4. **Handle competing updates atomically:** Shipping is performed with one `UPDATE` conditioned on the order ID and `status = 'pendiente'`. The affected-row count determines success; if no row is updated, return 404 if the order is absent, otherwise return 409, with no status change from this attempt.
5. **Show update failures:** When a shipping request fails, the UI presents a visible, clear error message to the user.
6. **Refresh after every attempt:** The UI reloads the order table after both successful and failed shipping attempts, so it does not retain stale order state.
7. **Close database connections:** Each SQLite connection is closed after use, including when queries or updates raise an error.
8. **Keep scope limited:** Do not add an empty-list message, missing-table handling for an unrun seed, seed fixes, refactoring, dependencies, or tests. Record those out-of-scope issues without changing their behavior.
