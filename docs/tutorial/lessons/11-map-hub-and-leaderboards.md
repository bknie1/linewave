# Lesson 11: Map Hub and Leaderboards

Branch: `lesson/11-hub`

Goal: shared maps and scores through a mock API that becomes the contract for the Go server.

Read first: react.dev "You Might Not Need an Effect" (the fetching section). TanStack Query docs: Quick Start, Query Keys, Important Defaults, Paginated Queries, Mutations, Invalidations from Mutations, Optimistic Updates. MSW docs: Getting Started for the browser and for Node. react.dev Reference: `useActionState`, `useFormStatus`.

## Build

1. MSW handlers in `src/mocks/handlers.ts`, shared by the browser worker and the test server. Each waits 200 to 800 ms.
   - `GET /api/maps?q=&difficulty=&sort=&dir=&page=` returns `{ items, total, page, pageSize }`.
   - `GET /api/maps/{id}`
   - `POST /api/maps` accepts a map file only. It rejects bodies over 256 KB, unknown fields, and anything failing the schema, with field errors.
   - `POST /api/maps/{id}/favorite` and `DELETE /api/maps/{id}/favorite`
   - `GET /api/maps/{id}/scores` and `POST /api/maps/{id}/scores`
   - Write the contract down in `docs/api.md`: paths, query parameters, bodies, status codes, error shape.
2. The wrong way first, on a scratch branch you do not merge: load the map detail with `useEffect` and `fetch`, click between maps fast, and find the bug. Keep notes for the PR body.
3. The right way:
   - `QueryClientProvider` at the root and a query key factory in one file.
   - The hub is a searchable, filterable, sortable, paginated table. Its state lives in the URL through `useSearchParams`. It uses `placeholderData: keepPreviousData` so paging does not flash empty.
   - Map detail shows the map's info, its top scores, and whether the player has the matching song.
   - Publish from the editor with `useMutation`; on success, invalidate the hub lists.
   - Favorite is optimistic: `onMutate` updates the cache, `onError` rolls back, `onSettled` invalidates.
   - The end-of-song screen submits a score through `<form action>` with `useActionState`, and the submit button reads `useFormStatus`.
4. A results count in a polite live region, plus empty and error states.

## Tests

- A test setup that starts the MSW server, resets handlers after each test, and gives each test a fresh `QueryClient` with retries off.
- Searching, filtering, sorting, and paging update the URL and the rows.
- A `server.use` override returning 500 shows the error state.
- A failed favorite rolls back.
- Publishing invalidates the list, and the new map appears.

## PR questions

1. What was the bug in the `useEffect` version, and what does TanStack Query handle that your fix did not?
2. What is the difference between `staleTime` and `gcTime`, and what did you set?
3. When would you use `useActionState` over React Hook Form, and the reverse?

