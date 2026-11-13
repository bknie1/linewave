# Lesson 2: UI Primitives and a Static Board

Branch: `lesson/02-primitives`

Goal: the small components every later lesson builds on, and the board drawn from data.

Read first (react.dev, Learn): Conditional Rendering, Rendering Lists, Keeping Components Pure. Reference: `ref` as a prop (React 19 release notes).

## Build

1. `Button`
   - Extends `ComponentPropsWithoutRef<'button'>`, plus `variant: 'primary' | 'secondary'` (default `'secondary'`).
   - `type` defaults to `"button"`, and a caller can still override it.
   - Merges a caller's `className` with your own classes.
   - Accepts `ref` as a plain prop so a parent can focus it.
2. `Stat`: a label and a value for score, combo, and lines. Pick the semantic markup and defend it in the PR.
3. `Notice`: `tone: 'info' | 'success' | 'warning' | 'error'`, `title`, `children`. Decide which tones get `role="alert"`, which get `role="status"`, and which get none. The tone must not rely on color alone.
4. Types in `src/game/types.ts`: a `CellKind` union (empty plus one value per piece in your set) and `Board` as a read-only 2D array of `CellKind`.
5. `src/fixtures/boards.ts`: two hand-written boards, one empty and one mid-game with a few filled rows.
6. `Cell` and `Board`
   - `Board` renders rows of `Cell`s from a `Board` value.
   - Each piece kind gets a color and a pattern or glyph, so the board reads without color.
   - The board container has `role="img"` and an `aria-label` that states its size. The cells are hidden from assistive tech. Lesson 13 adds announcements.
7. `App` shows the mid-game board with `Panel`s for Next, Hold, and Score beside it.

## Tests

- `Button`: defaults to `type="button"`; passes `disabled` through; calls `onClick` once with `user-event`; a parent can focus it through `ref`.
- `Notice`: the role you chose for each tone.
- `Board`: renders the right number of rows and cells; a filled cell carries its kind (pick the assertion and justify it).

## PR questions

1. The board uses array indexes as keys. Why is that correct here, when lesson 4 says avoid them?
2. In `Button`, does `{...rest}` go before or after your own props, and what changes if you flip it?
3. Why does the board take its cells as a prop instead of creating them inside?

