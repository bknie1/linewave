# Lesson 2: Tailwind, shadcn/ui, and a Static Board

Branch: `lesson/02-primitives`

Goal: a styling system and a component library in place, a few primitives of your own, and the board drawn from data.

Read first (react.dev, Learn): Conditional Rendering, Rendering Lists, Keeping Components Pure. Reference: `ref` as a prop (React 19 release notes). Tailwind docs: Installing with Vite, Styling with utility classes, Theme variables, Detecting classes in source files. shadcn/ui docs: Introduction, Installation for an existing Vite project, Theming.

## Build

1. Tailwind
   - Install `tailwindcss` and `@tailwindcss/vite`, and add the plugin to `vite.config.ts`.
   - Add the `@` import alias to `tsconfig.json`, `tsconfig.app.json`, and `vite.config.ts`, as the shadcn/ui Vite guide shows.
   - Define your palette from `docs/design.md` as theme variables in `@theme`: one `--color-piece-*` per piece kind, plus board and beat-cue colors. These are your tokens, the same idea as an ITCSS settings layer.
2. shadcn/ui
   - Run `npx shadcn@latest init` in the existing project and choose Radix as the base.
   - Add `button`: `npx shadcn@latest add button`. The CLI copies the source into `src/components/ui/button.tsx`. You own that file now.
   - Read it before you use it, and be able to explain:
     - `cn()` in `src/lib/utils.ts`: `clsx` joins class names, and `tailwind-merge` resolves conflicts so a caller's `px-6` beats the default `px-4`.
     - `cva`: variants and sizes as a typed lookup, with `VariantProps` feeding the props type.
     - `asChild` and `Slot`: rendering the button's styles and behavior onto a child element such as a link.
     - How it takes `ref` and the native button props.
   - Add a `beat` variant yourself, for the on-beat buttons in the menus.
3. `Stat`, hand-built: a label and a value for score, combo, and lines. Pick the semantic markup and defend it in the PR.
4. `Notice`, hand-built: `tone: 'info' | 'success' | 'warning' | 'error'`, `title`, `children`. Decide which tones get `role="alert"`, which get `role="status"`, and which get none. The tone must not rely on color alone.
5. Types in `src/game/types.ts`: a `CellKind` union (empty plus one value per piece in your set) and `Board` as a read-only 2D array of `CellKind`.
6. `src/fixtures/boards.ts`: two hand-written boards, one empty and one mid-game with a few filled rows.
7. `Cell` and `Board`
   - `Board` renders rows of `Cell`s from a `Board` value, laid out with CSS grid utilities.
   - Each piece kind gets a color and a pattern or glyph, so the board reads without color.
   - Map each `CellKind` to a complete class string, such as `'bg-piece-a'`. Never build one from parts like `` `bg-piece-${kind}` ``; Tailwind only generates classes it finds whole in your source.
   - The board container has `role="img"` and an `aria-label` that states its size. The cells are hidden from assistive tech. Lesson 13 adds announcements.
8. `App` shows the mid-game board with `Panel`s for Next, Hold, and Score beside it. Style `Panel` from lesson 1 with utilities now.

## Tests

- `Button`: defaults to `type="button"` or tell the reviewer why the shadcn version does not, and fix it if it should; passes `disabled` through; calls `onClick` once with `user-event`; a parent can focus it through `ref`; a caller's `className` overrides a conflicting default.
- `Notice`: the role you chose for each tone.
- `Board`: renders the right number of rows and cells; a filled cell carries its kind (pick the assertion and justify it).

## PR questions

1. The board uses array indexes as keys. Why is that correct here, when lesson 4 says avoid them?
2. What does `tailwind-merge` do that plain string joining does not, and what bug would you see without it?
3. shadcn/ui copies source into your repo instead of installing a package. What do you gain, and what do you now have to maintain?
4. Why must Tailwind class names appear whole in the source?
