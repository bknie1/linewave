# Lesson G1: Map Validator CLI

Branch: `lesson/g1-mapcheck`
Comes after: React lesson 10.

Goal: your first Go program, small and complete: a command-line tool that validates map files by the same rules as the Zod schema.

Read first: A Tour of Go (Basics, Methods and interfaces). Effective Go (Names, Errors). go.dev "Tutorial: Create a Go module" and "Add a test".

## Build

1. Upgrade Go from 1.23 to the current 1.27 release and confirm with `go version`.
2. A module in `server/` at the repo root: `go mod init github.com/bknie1/linewave/server`.
3. `server/internal/maps`
   - A `MapFile` struct with JSON tags that mirrors the Zod schema from lesson 10.
   - `Parse(r io.Reader) (MapFile, error)` that rejects unknown fields.
   - `Validate(m MapFile) error` with the same rules as the Zod custom checks. Collect every problem and return them together with `errors.Join`. Each problem names its field path.
4. `server/cmd/mapcheck`: `mapcheck <file>...` prints `ok` or the problems for each file and exits nonzero if any file fails.
5. `go vet ./...` is clean and `gofmt` changes nothing.

## Tests

- Table-driven tests over every file in `fixtures/maps/`, the same fixtures the Vitest tests use. Valid files pass; each invalid file fails with the expected field path.
- One test for unknown fields.
- Subtests with `t.Run`, named after the fixture.

## PR questions

1. How do Go errors differ from C# exceptions, and what did `errors.Join` give you?
2. What do the JSON struct tags do, and what happens to a field with no tag?
3. Where do the Zod schema and this Go validator disagree, if anywhere, and how would you stop them drifting?

