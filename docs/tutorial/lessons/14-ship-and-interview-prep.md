# Lesson 14: Ship and Interview Prep

Branch: `lesson/14-ship`

Goal: a live game, a README that reads as a case study, and practice under interview conditions.

Read first: Vite docs (Deploying a Static Site, GitHub Pages). Next.js docs (App Router: Server and Client Components, Server Actions and Mutations).

## Part A: Ship (the PR)

1. The front end on GitHub Pages at `bknie1.github.io/linewave`. Set Vite's `base` and handle deep links with a 404 fallback page.
2. The front end talks to the Go API from G5. If the API is unreachable, the hub and scores fall back to MSW behind a visible "Offline demo" banner, and play still works.
3. CI on every PR: the npm checks, plus `go vet` and `go test -race` for `server/`.
4. About page: audio stays on the player's device; maps contain timing data only; starter track credits and licenses; an honest accessibility statement.
5. README in your voice: what the game is, why you built it, the stack, how to run it, and three decisions you would defend in an interview (the pure engine, the audio clock as an external store, the contract-first API).

## Part B: Next.js evening (separate folder, not merged)

In `C:\My Projects\linewave-next`, a throwaway Next.js app:

1. The map hub list as a Server Component that reads the fixtures directly. No `useEffect`, no TanStack Query.
2. The search box as a `'use client'` island.
3. A server action that posts a score.
4. Half a page in `docs/next-notes.md` in this repo: what moved to the server, what stayed on the client, and how it differs from your Vite app.

## Part C: Drills

Timed, 30 to 45 minutes each, no AI, no project code open, talking out loud. The reviewer can play interviewer.

React:

1. Typeahead with debounce that ignores stale responses.
2. Accessible tabs with arrow-key navigation.
3. Modal on `<dialog>` with focus return.
4. Paginated list with loading, empty, and error states.
5. Todo list with add, toggle, delete, and filter.
6. `useLocalStorage` hook with tests.
7. Fix-the-bug: stale closure, missing key, and an effect that should be derived state.

Go:

8. A worker pool that processes jobs and stops cleanly on `context` cancellation.
9. An HTTP handler with validation and a table-driven `httptest` test.

Then a mock interview: 15 questions drawn at random from both question lists in the tutorial README, answered without notes.

## Done when

- The game is live and every flow works on it.
- Every drill done once inside its time.

