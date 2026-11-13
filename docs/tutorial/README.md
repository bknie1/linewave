# Linewave Tutorial

This tutorial builds Linewave from an empty repo, one lesson at a time. Linewave is an original rhythm block-clearing game with a beatmap editor. The front end is React and TypeScript, and a Go API serves shared maps and leaderboards. Every lesson ends in one pull request, so the PR history of this repo shows the learning as it happened.

The lessons are in [lessons/](lessons/). The starter music plan is in [starter-tracks.md](starter-tracks.md).

## Ground rules

- The learner writes every line of project code. Review happens after the code is written, never before.
- AI completion in the editor stays off for this project. Docs and MDN are fine, and react.dev is the primary reference.
- Function components and hooks only, with TypeScript in strict mode throughout. The one class component is the error boundary in lesson 7, written once so you know why it is a class.
- Fixture data (fictional map entries, scores, player names) may be generated. It teaches nothing about React or Go.
- Player audio is never uploaded, hosted, shared, or downloaded from another site. The only music in the repo is the starter set, which is CC0 or CC BY with credits.

## Lesson workflow

1. Start from an up-to-date `main`: `git switch main`, `git pull`, then `git switch -c lesson/NN-short-name`.
2. Build to the lesson spec. Commit as often as you like.
3. Before opening the PR: `npm test`, `npx tsc -b`, and `npm run lint` all pass. For Go lessons, `go vet ./...` and `go test -race ./...` pass and `gofmt` leaves nothing to change.
4. Push and open the PR. Title: `Lesson NN: <name>`. Body: what you built, the answers to the lesson's questions, and anything you got stuck on.
5. The PR gets a code review sorted as must fix, should fix, and worth knowing.
6. Push fixes to the same branch until the review is approved.
7. Merge with a merge commit so the review rounds stay in history. The next lesson starts from the new `main`.

The scaffold from `create-vite` is the only code committed directly to `main`.

## Toolchain (checked against npm on 2026-10-16)

| Piece | Version | Why |
|---|---|---|
| React, React DOM | 19.3.1 | Current stable. |
| Vite (`create-vite` 9.2.1, `react-ts` template) | 8.3.4 | A plain client-side app. It teaches React itself, with no framework layer in the way. |
| TypeScript | 7.0 (whatever the template pins) | Strict mode on. |
| Oxlint (ships with the template) | template default | Turn on `react/rules-of-hooks` and `react/exhaustive-deps`. The dependency lint is part of the lesson. |
| Vitest, jsdom | 5.0.5, 30.1.3 | A test runner that shares Vite's config. |
| React Testing Library, user-event, jest-dom | 16.3.3, 14.6.8, 7.0.1 | The standard approach to testing React. |
| React Router | 8.4.0, declarative mode | The router most teams use. The package is `react-router`, no longer `react-router-dom`. |
| TanStack Query | 5.104.3 | Server state. |
| MSW | 3.0.3 | A mock API that defines the contract before the Go API exists. It stays in the front-end tests. |
| React Hook Form, Zod | 7.90.0, 4.6.7 | Lesson 10, after the hand-built settings form in lesson 8. |
| Web Audio API | browser built-in | Decoding, the playback clock, and beat analysis. No audio library. |
| React Compiler | 1.0 | Off until lesson 12, so manual memoization comes first. |
| Styling | Sass on BEMIT, your own tokens | Interviews do not test CSS method choice. |
| Go | 1.27 | Standard library first. The one dependency is the pure-Go SQLite driver `modernc.org/sqlite`, so no C compiler is needed. |

Why Vite over Next.js: Next.js is the most common React framework, but it wraps React in server rendering, Server Components, and its own routing and caching rules. Learning those at the same time as hooks blurs which rules belong to React. Vite first gives you React as most teams write their client code. Lesson 14 spends one evening on Server Components and Next.js.

## The game

- Blocks fall on the beat of a song. Moves made on the beat build a combo multiplier, and line clears on the beat score a bonus.
- An original look and piece set: a board size other than 10 by 20, original piece shapes, and an original visual style. Classic falling-block games are a reference point only.
- Players supply their own audio files. The browser decodes and analyzes them with the Web Audio API on the player's device. Player audio is never uploaded, hosted, or shared.
- A beatmap editor: load a song, see its waveform, auto-detect beats as a starting point, tap or adjust beats, set sections (speed changes, special pieces), preview, undo and redo, then export.
- Maps are timing data only. A map identifies its song by a SHA-256 hash, title, and duration, so it works only with the player's own copy of the file. The app shares maps, never music.
- A starter set of CC0 and CC BY tracks in dubstep, drum and bass, and electro house, each with a map, so the game plays on first load. Credits are on the about page.
- A shell around the game: menu, track library, map hub (search, filter, sort, and paginate shared maps), leaderboards, settings, and about. The hub and leaderboards start on an MSW mock API, which defines the contract. A Go API then implements that contract. MSW stays in the front-end tests.
- Accessibility: WCAG 2.2 AA for the shell and editor. The Game Accessibility Guidelines for play: remappable keys, a visual beat cue that works with sound off, relaxed timing windows, pause anytime, reduced motion, and color-blind palettes with patterns on pieces.

Out of scope: accounts, any hosting of player audio, and downloading audio from any site.

## React lessons

One lesson, one PR. An evening is two to three focused hours.

| # | Lesson | Idioms | Evenings |
|---|---|---|---|
| 1 | [Scaffold, test setup, first component](lessons/01-scaffold-and-first-component.md) | JSX, function components, typed props, `children`, the first RTL test, `getByRole` | 1 |
| 2 | [UI primitives and a static board](lessons/02-primitives-and-static-board.md) | Composition, extending native props, `ref` as a prop, rendering a 2D grid with stable keys | 1 to 2 |
| 3 | [Game engine as pure TypeScript](lessons/03-game-engine.md) | Discriminated unions, immutable updates, a reducer with no React in it, unit tests | 2 |
| 4 | [Playable board](lessons/04-playable-board.md) | `useReducer`, keyboard input in event handlers, state as a snapshot, derived values (ghost piece, next queue) | 1 to 2 |
| 5 | [Game loop](lessons/05-game-loop.md) | `requestAnimationFrame` in an effect with cleanup, `useRef` for time, StrictMode double run, pause on hidden tab | 1 to 2 |
| 6 | [Audio and the beat](lessons/06-audio-and-the-beat.md) | Accessible file drop, Web Audio API, `useSyncExternalStore` over the audio clock, custom hooks, timing judgment | 2 |
| 7 | [Routing and shell](lessons/07-routing-and-shell.md) | Layout routes, `Outlet`, `NavLink`, `useParams`, `lazy` and `Suspense`, a class error boundary | 1 to 2 |
| 8 | [Settings and context](lessons/08-settings-and-context.md) | Context versus props versus composition, memoized provider values, persisted settings, a hand-built controlled form, key remapping | 1 to 2 |
| 9 | [Map editor timeline](lessons/09-editor-timeline.md) | Canvas through a ref, an effect that draws, undo and redo with a history reducer, beat detection as a pure function | 2 to 3 |
| 10 | [Map format, import and export](lessons/10-map-format.md) | Zod schema and inferred types, React Hook Form, uncontrolled inputs, `crypto.subtle`, accessible validation errors | 1 to 2 |
| 11 | [Map hub and leaderboards](lessons/11-map-hub-and-leaderboards.md) | MSW, TanStack Query, query keys, URL search params, `useMutation`, invalidation, optimistic updates, `useActionState` | 2 |
| 12 | [Performance](lessons/12-performance.md) | Profiler during play, `memo`, moving per-frame visuals out of React state, `useDeferredValue`, React Compiler | 1 |
| 13 | [Accessibility and test pass](lessons/13-accessibility-and-test-pass.md) | axe in tests, Game Accessibility Guidelines items, the WCAG 2.2 dragging alternative, live regions, MSW per-test overrides | 1 to 2 |
| 14 | [Ship and interview prep](lessons/14-ship-and-interview-prep.md) | Production build, GitHub Pages, a README case study, one Next.js evening, timed drills | 2 to 3 |

React total: 19 to 28 evenings.

## Go lessons

The Go track builds the real API behind the map hub and leaderboards, in a `server/` folder in this repo. Go comes in once the front end has a contract to implement.

| # | Lesson | Go idioms | After | Evenings |
|---|---|---|---|---|
| G1 | [Map validator CLI](lessons/G1-map-validator-cli.md) | Modules, structs and JSON tags, errors as values, table-driven tests | Lesson 10 | 1 to 2 |
| G2 | [HTTP API, in memory](lessons/G2-http-api-in-memory.md) | `net/http` routing patterns, handlers, interfaces at the consumer, `httptest` | Lesson 11 | 2 |
| G3 | [SQLite persistence](lessons/G3-sqlite-persistence.md) | `database/sql`, a pure-Go SQLite driver, embedded migrations, `context` timeouts | G2 | 1 to 2 |
| G4 | [Hardening](lessons/G4-hardening.md) | Body size limits, validation, CORS, rate limiting with a mutex, `log/slog`, graceful shutdown, `go test -race` | G3 | 1 to 2 |
| G5 | [Wire up and deploy](lessons/G5-wire-up-and-deploy.md) | The front end switches from MSW to the API by environment, a Dockerfile, contract tests shared with the MSW handlers | G4 | 1 to 2 |

Go total: 6 to 10 evenings. Together: 25 to 38 evenings, about five to seven weeks at five evenings a week.

Order: React 1 to 10, G1, React 11, G2 to G5, React 12 to 14.

## Live-coding drills

Lesson 14 runs these timed, 30 to 45 minutes each, with no AI and no project code open, talking out loud.

1. Typeahead search with debounce that ignores stale responses.
2. Accessible tabs with arrow-key navigation.
3. A modal on the native `<dialog>` element with focus return.
4. A paginated list from a fetch, with loading, empty, and error states.
5. A todo list with add, toggle, delete, and filter, all state immutable.
6. A `useLocalStorage` custom hook with tests.
7. Fix-the-bug: a component with a stale closure, a missing key, and an effect that should be derived state.
8. Go: a worker pool that processes jobs and stops cleanly on `context` cancellation.
9. Go: an HTTP handler with validation and a table-driven `httptest` test.

## React questions worth answering in one line

Say them in your own words; a memorized answer is easy to spot.

**Rendering**

- What triggers a re-render? A state update in the component, a parent re-rendering, or a change in a context it reads.
- What is reconciliation? React compares the new element tree with the previous one by type and key, then commits the minimum DOM changes.
- Why are keys needed? They give list items a stable identity so React keeps the right state and DOM with the right item.
- Why not use the array index as a key? Indexes shift on insert, delete, or reorder, so state attaches to the wrong item.
- How do you reset a component's state? Change its `key`; React treats it as a new component and remounts it.
- Why not define a component inside another component? It becomes a new type on every render, so it remounts and loses its state each time.
- What does StrictMode do in development? It double-invokes renders and mounts, unmounts, and remounts effects to surface impure renders and missing cleanup.

**State**

- When do you lift state? When two components need the same data, move it to their closest common parent and pass it down.
- What is derived state, and where does it go? Anything computable from props or state is computed during render, never stored in state.
- Why use the updater form `setCount(c => c + 1)`? State is a snapshot for that render, so the updater reads the latest queued value.
- Is `setState` synchronous? No; it schedules a render, and React batches every update in the same event into one render.
- Why never mutate state? React compares by reference, so a mutated object looks unchanged and the UI goes stale.
- `useState` or `useReducer`? A reducer when several values change together or transitions depend on the action type; it is also easy to unit test.
- What is `useRef` for? A value that persists across renders without causing one: DOM nodes, timer IDs, the latest value for a callback.
- Controlled versus uncontrolled inputs? Controlled takes its value from state with `onChange`; uncontrolled lets the DOM hold the value, read through a ref or `FormData`.

**Effects**

- What is `useEffect` for? Synchronizing with a system outside React: a subscription, a timer, a browser API, a non-React widget.
- When is an effect the wrong tool? Transforming data for render, handling a user event, resetting state on a prop change, or fetching data.
- What goes in the dependency array? Every reactive value the effect reads; if the lint rule complains, fix the code and leave the array complete.
- When does cleanup run? Before the effect runs again and when the component unmounts.
- What is wrong with fetching in `useEffect`? Race conditions, no caching or deduplication, request waterfalls, and hand-rolled loading and error state.
- `useEffect` or `useLayoutEffect`? Layout effect only when you must measure or change the DOM before the browser paints.
- What is a stale closure? A callback or effect that captured old state because it was created in an earlier render.

**Context and composition**

- When is context right? For values many components need that change rarely: theme, current user, locale.
- What is the cost of context? Every consumer re-renders when the provider value changes, so memoize the value and split contexts.
- What should you try before context? Composition: pass components as `children` so intermediate layers do not need the prop.
- Is context a state manager? No; it passes a value down. State still lives in `useState` or `useReducer` above the provider.
- What is a custom hook? A function starting with `use` that reuses stateful logic; each call gets its own state.
- What are the rules of hooks? Call hooks at the top level of components or custom hooks, in the same order every render.

**Performance**

- What do `useMemo` and `useCallback` do? Cache a value or a function between renders until a dependency changes.
- When should you use them? When a measured computation is slow, or when a stable reference matters to a memoized child or an effect dependency.
- What does `React.memo` do? Skips re-rendering a component when its props are shallowly equal; useless if you pass new objects each render.
- What does React Compiler change? It inserts memoization automatically at build time, so most manual `useMemo` and `useCallback` goes away.
- `useTransition` or `useDeferredValue`? Both mark work as non-urgent so input stays responsive; transition wraps the update, deferred value wraps the value.
- How do you split code? `lazy()` plus `Suspense`, usually at the route level.
- How do you render 10,000 rows? Virtualize with a windowing library so only visible rows mount.

**Data**

- Server state versus client state? Server state is a cache of remote data that goes stale; client state is UI state you own.
- What is a query key? The identity of cached data, including its parameters; change the key and you get a different cache entry.
- `staleTime` versus `gcTime`? `staleTime` is how long data counts as fresh; `gcTime` is how long unused data stays in memory.
- How do you update the UI after a mutation? Invalidate the affected query keys so they refetch, or write the result into the cache.
- How do you do an optimistic update? Update the cache in `onMutate`, roll back in `onError`, and invalidate in `onSettled`.
- What does `Suspense` do for data? Shows a fallback while a child waits on data or code, so loading UI lives in one place.
- What does an error boundary catch? Errors thrown while rendering its children; it does not catch errors in event handlers or async code.

**React 19 and frameworks**

- What are Server Components? Components that render only on the server, ship no JavaScript, and cannot use state or effects.
- What does `'use client'` mean? It marks the boundary where components start running on the client and can be interactive.
- What are actions and `useActionState`? A function passed to a form's `action` prop; `useActionState` tracks its pending state and result.
- What is `useOptimistic`? It shows a temporary optimistic value while an async action is in flight.
- What is `use`? It reads a promise or a context during render and can be called conditionally.
- Do you still need `forwardRef`? No; in React 19 `ref` is a regular prop on function components.
- What is hydration? Attaching React to server-rendered HTML; a mismatch happens when server and client render different output.

**TypeScript with React**

- How do you type props? A `type` for the props object, `children: React.ReactNode` when needed, and `ComponentPropsWithoutRef<'button'>` for wrappers.
- How do you type an event handler? `React.ChangeEvent<HTMLInputElement>`, or let inference type it inline.
- How do you type `useState` with no initial value? Give the generic: `useState<MapEntry | null>(null)`.

**Testing**

- What does React Testing Library encourage? Testing what the user sees and does, never the component's internals.
- Which query first? `getByRole` with an accessible name, then label and text queries; test IDs last.
- `getBy`, `queryBy`, or `findBy`? `getBy` asserts presence, `queryBy` asserts absence, `findBy` waits for async content.
- Why `user-event` over `fireEvent`? It simulates real interaction sequences such as focus, keydown, input, and keyup.
- How do you mock the network? MSW intercepts requests, so components run their real fetching code.

**Accessibility**

- Does React make an app accessible? No; semantic HTML is the foundation, and React adds focus management and dynamic-update work on top.
- What is `useId` for? Stable unique IDs for `htmlFor`, `aria-describedby`, and similar; never for list keys.
- What must a single-page app handle that a server-rendered site gets free? Focus and an announcement on route change, and updated document titles.
- How do you announce async results? A polite live region that already exists in the DOM before its text changes.

## Go questions worth answering in one line

- What is a goroutine? A function running concurrently, scheduled by the Go runtime onto a small pool of OS threads; starting one is cheap.
- Channels or a mutex? Channels to hand data or ownership between goroutines; a mutex to guard shared state in place.
- What does an unbuffered send do? It blocks until a receiver takes the value.
- What does `select` do? Waits on several channel operations at once; a `default` case makes it non-blocking, and `time.After` adds a timeout.
- How do you prevent goroutine leaks? Give every goroutine an exit path, usually through `context` cancellation or a closed channel.
- What is `context.Context` for? Deadlines, cancellation, and request-scoped values across API boundaries; it is the first parameter by convention.
- How are errors handled? As returned values; wrap with `fmt.Errorf("...: %w", err)` and check with `errors.Is` and `errors.As`. Panic is for programmer bugs.
- How do interfaces work? Types satisfy them implicitly; define small interfaces where they are consumed.
- What is the nil interface trap? An interface holding a typed nil pointer is itself non-nil.
- Value or pointer receiver? Pointer when the method mutates the receiver or the struct is large; keep one style per type.
- What is a slice? A pointer to a backing array plus length and capacity; `append` may reallocate, and two slices can share one array.
- Are maps safe for concurrent use? No; concurrent writes need a mutex or `sync.Map`.
- When does `defer` run? When the function returns, last in first out, with arguments evaluated at the `defer` line.
- What is struct embedding? Composition that promotes the embedded type's fields and methods to the outer type.
- When do you use generics? When the same logic applies across types, such as a typed collection helper; interfaces still cover behavior.
- How do you test in Go? Table-driven tests with `t.Run` subtests, `httptest` for handlers, and `go test -race` to catch data races.
- What are `go.mod` and `go.sum`? `go.mod` declares the module path and dependencies; `go.sum` records their checksums.
