# Lesson G2: HTTP API, In Memory

Branch: `lesson/g2-api`
Comes after: React lesson 11.

Goal: a Go server that implements the contract in `docs/api.md`, backed by memory for now.

Read first: `net/http` package docs (`ServeMux` patterns with methods and wildcards, `Request.PathValue`). `net/http/httptest` package docs. go.dev blog "Routing Enhancements for Go 1.22". Effective Go (Interfaces, Concurrency).

## Build

1. `server/internal/store`
   - A `Store` interface defined where it is used, with only the methods the handlers need: list maps with query, get map, create map, set favorite, list scores, add score.
   - `MemoryStore` guarded by a `sync.RWMutex`.
2. `server/internal/api`
   - A `ServeMux` with routes such as `GET /api/maps/{id}`.
   - Handlers decode JSON, call the store, and encode JSON. One helper for writing JSON and one for writing errors in the contract's error shape.
   - Query parameter parsing for search, filter, sort, and page, with defaults and limits.
   - Publishing reuses `maps.Parse` and `maps.Validate` from G1.
3. `server/cmd/api`: the server's `main`, with the port from an environment variable.
4. Vite's `server.proxy` sends `/api` to the Go server in development, with MSW turned off by an environment variable.

## Tests

- Table-driven `httptest` tests for every route: status codes, bodies, and the error shape.
- Paging and sorting edge cases: page past the end, an unknown sort key.
- A concurrent test that adds scores from many goroutines, run with `go test -race`.

## PR questions

1. Why define `Store` in the package that uses it instead of next to `MemoryStore`?
2. Why `RWMutex` over `Mutex` here, and when would it be the wrong choice?
3. What happens to a request when a handler panics, and what would you add for production?

