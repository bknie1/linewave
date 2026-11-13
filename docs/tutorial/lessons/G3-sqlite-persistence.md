# Lesson G3: SQLite Persistence

Branch: `lesson/g3-sqlite`

Goal: the same API, now backed by a database, swapped in behind the `Store` interface.

Read first: go.dev "Accessing a relational database" tutorial. `database/sql` package docs (`QueryContext`, `ExecContext`, `Tx`). `embed` package docs. `context` package docs.

## Build

1. Add `modernc.org/sqlite`, a pure-Go driver, so no C compiler is needed.
2. Migrations as numbered `.sql` files in `server/internal/store/migrations/`, embedded with `//go:embed` and applied in order at startup, with a table that records which ran.
3. `SQLStore` implements `Store`:
   - Every query takes the request's `context.Context`.
   - Search, filter, sort, and page in SQL with parameters. The sort column comes from a fixed allow-list, never from the request string.
   - Creating a map and its first favorite count runs in a transaction.
4. The server picks `MemoryStore` or `SQLStore` from configuration.
5. Request handlers set a timeout on the context so a slow query cannot hang a request.

## Tests

- Run the G2 handler tests against both stores with the same table, so the contract holds for each.
- Migrations apply once and are idempotent on restart.
- A cancelled context stops a query with `context.Canceled`.
- A sort value outside the allow-list is rejected.

## PR questions

1. Why must the sort column come from an allow-list when every other value is a parameter?
2. What does passing `context` into every query buy you?
3. Running the same tests against both stores: what did it catch?

