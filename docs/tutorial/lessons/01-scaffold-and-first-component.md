# Lesson 1: Scaffold, Test Setup, First Component

Branch: `lesson/01-panel`

Goal: a running Vite app on `main`, a design note for the game, and a PR that adds the test setup and one component with tests, all written by you.

Read first (react.dev, Learn): Your First Component, Writing Markup with JSX, Passing Props to a Component.

## Part A: Scaffold, committed straight to main

This is the only work that skips a PR. The repo already holds this tutorial, so the scaffold goes into the existing clone. If your global git identity is not the one you want on this repo, set `user.name` and `user.email` with `git config` inside the clone first.

```
cd "C:\My Projects\linewave"
git pull
npm create vite@latest . -- --template react-ts
```

create-vite warns that the folder is not empty. Choose "Ignore files and continue", never "Remove existing files". The template writes its own README, so put the repo's back, then install and run:

```
git restore README.md
npm install
npm run dev
```

Confirm the starter page loads, then stop the server.

```
git add .
git commit -m "Scaffold from the create-vite react-ts template"
git push
```

## Part B: The lesson, on a branch

```
git switch -c lesson/01-panel
```

### 1. Design note

Write `docs/design.md` in the repo, in your own words, half a page:

- The name and a one-line pitch.
- Board size (anything but 10 by 20) and the piece set (your own shapes).
- The visual direction. Your own influences are fair game.
- The rhythm rule: what happens on the beat, and what an on-beat move earns.
- Timing windows to start with, for example Perfect within 50 ms and Good within 100 ms.

### 2. Read before you delete

Be able to say what each of these does:

- `index.html`: why the script tag points at a `.tsx` file.
- `src/main.tsx`: what `createRoot` and `StrictMode` are doing.
- `src/App.tsx`: the `useState` call and the `onClick` handler.
- `tsconfig.app.json`: confirm `strict` is on.
- The Oxlint config: add `react/exhaustive-deps` as an error.

Then delete the starter content in `App.tsx` and `App.css` so the app renders an empty `<main>`.

### 3. Test setup

Install as dev dependencies: `vitest`, `jsdom`, `@testing-library/react`, `@testing-library/user-event`, `@testing-library/jest-dom`.

Configure it yourself from the Vitest and jest-dom docs. The setup needs:

- The `jsdom` test environment.
- A setup file that registers the jest-dom matchers for Vitest.
- A `test` script in `package.json`.
- Globals off; import `describe`, `it`, and `expect` explicitly in each test.

### 4. The component

Write `src/components/Panel.tsx`. The side panels (Next, Hold, Score) use it later.

- Props: `title` (string, required), `headingLevel` (2 or 3, optional, defaults to 2), `children` (optional).
- Renders a `<section>` containing a heading at the given level with the title, then the children.
- The section is labelled by its heading, so a screen reader lists it as a named region. Use `useId` to connect them.
- Props get a named `type`. No `React.FC`.

Use it once in `App.tsx` with the title "Next" and placeholder text.

### 5. The tests

Write `src/components/Panel.test.tsx`:

1. It renders a level 2 heading with the title when no level is given.
2. It renders a level 3 heading when `headingLevel={3}`.
3. It renders a region whose accessible name is the title, and the children are inside that region.

Use `getByRole` for all three. Use `within` for the third.

## PR checklist

- `npm test`, `npx tsc -b`, and `npm run lint` pass.
- PR title `Lesson 01: Panel, test setup, design note`.
- PR body answers these, one sentence each:
  1. `Panel` runs again when its parent re-renders. What happens to a variable declared inside it?
  2. Why does `headingLevel` use the type `2 | 3` instead of `number`?
  3. What did TypeScript make you do to render a heading tag chosen by a prop?

