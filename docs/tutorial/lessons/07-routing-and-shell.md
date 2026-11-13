# Lesson 7: Routing and Shell

Branch: `lesson/07-routing`

Goal: the game becomes an app with screens, and failures land on a recovery screen instead of a blank page.

Read first: React Router docs, declarative mode (Installation, Routing, Navigating, URL Values). react.dev: `lazy`, `Suspense`, and the "Catching rendering errors with an error boundary" section of the `Component` reference.

## Build

1. Install `react-router`. Declarative mode: `BrowserRouter` and `Routes`.
2. A layout route for menus: skip link, header with the game name, primary nav using `NavLink`, `<main>` with an `Outlet`.
3. Routes:
   - `/` menu
   - `/library` the songs loaded this session and the bundled starter tracks
   - `/play/:trackId` the game, full screen, outside the menu layout
   - `/editor` and `/editor/:mapId` placeholders for lesson 9
   - `/hub`, `/scores`, `/settings`, `/about` placeholders
   - `*` not found
4. `lazy()` for the editor and hub routes, wrapped in `Suspense` with an accessible loading message.
5. Write `ErrorBoundary` as a class component with `getDerivedStateFromError` and `componentDidCatch`. Wrap play and the editor in it. The fallback offers Back to menu and Try again.
6. Leaving play mid-song pauses and asks for confirmation.

## Tests

- Render routes inside `MemoryRouter` with `initialEntries`.
- The current nav link has `aria-current="page"`.
- `/play/unknown` shows a helpful message.
- An unknown route shows not found.
- A child that throws during render shows the fallback, and Try again recovers. Silence the expected console error in that test.

## PR questions

1. Why does play sit outside the menu layout route?
2. Name two kinds of errors your boundary does not catch, and where you handle each instead.
3. Why is the error boundary a class when nothing else is?

