# Lesson G4: Hardening

Branch: `lesson/g4-hardening`

Goal: the server behaves well when clients are careless or hostile.

Read first: `net/http` docs (`MaxBytesReader`, `Server` timeouts, `Server.Shutdown`). `log/slog` package docs. Go blog "Go Concurrency Patterns: Context". OWASP API Security Top 10, a skim.

## Build

1. Middleware, each a function that wraps an `http.Handler`:
   - Request ID, set on the context and returned in a response header.
   - Structured logging with `slog`: method, path, status, duration, request ID.
   - Panic recovery that logs and returns a 500 in the contract's error shape.
   - CORS that allows the GitHub Pages origin and localhost only.
2. Limits
   - `http.MaxBytesReader` at 256 KB on every body. Maps are timing data, so anything bigger is refused.
   - A body that looks like audio (by content type or leading bytes) is refused with a clear message. This enforces the project's no-audio rule on the server.
   - Score values are checked against the map's possible maximum.
3. A per-IP rate limiter written by hand:
   - A token bucket per client in a map guarded by a mutex.
   - A cleanup goroutine removes idle clients and stops when the server's context is cancelled.
   - Over the limit returns 429 with `Retry-After`.
4. `http.Server` with read-header, read, write, and idle timeouts.
5. Graceful shutdown: on interrupt, stop accepting requests, let in-flight requests finish within a deadline, then close the store.

## Tests

- A 300 KB body gets 413; an audio-looking body gets refused.
- The rate limiter allows a burst, then returns 429, then recovers after refill. Use an injected clock, not sleeps.
- Recovery middleware turns a panicking handler into a 500.
- Everything passes under `go test -race`.

## PR questions

1. How does the cleanup goroutine know to stop, and what leaks if it doesn't?
2. Why inject a clock into the rate limiter?
3. Which OWASP API risks does this lesson address, and which remain?

