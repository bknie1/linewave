# Lesson 8: Settings and Context

Branch: `lesson/08-settings`

Goal: player settings that every screen reads, held in context, edited through a hand-built form.

Read first (react.dev, Learn): Passing Data Deeply with Context, Scaling Up with Reducer and Context. Reference: `useContext`, `useSyncExternalStore`. Game Accessibility Guidelines (gameaccessibilityguidelines.com), the Basic level.

## Build

1. `useMediaQuery(query)` on `useSyncExternalStore` and `matchMedia`.
2. `SettingsProvider`
   - Settings: key bindings, audio latency offset in ms, timing mode (`'standard' | 'relaxed'`), reduced motion (`'system' | 'on' | 'off'`), palette (default, color-blind safe, high contrast), beat cue on or off, theme.
   - Persisted to `localStorage`, with safe defaults if the stored value is missing or malformed.
   - Reduced motion `'system'` resolves through `useMediaQuery('(prefers-reduced-motion: reduce)')`.
   - The context value is memoized. Settings and the update function may be split into two contexts; say whether you did and why.
   - `useSettings()` throws a clear error outside the provider.
3. Settings form at `/settings`, hand-built:
   - A draft copy in `useReducer`, with controlled inputs, Save, and Cancel.
   - Key remapping: activate a binding, press a key to capture it, Escape cancels. A key already bound elsewhere shows an error naming the conflict.
   - Errors use `aria-invalid` and `aria-describedby`. On save with errors, focus moves to an error summary.
4. Latency calibration: a metronome plays, the player taps along for 16 beats, and the average offset becomes the latency setting.
5. The game, the beat judgment, and the board styles read settings from context.

## Rules for this lesson

- Game state does not go into context.
- No prop is passed through more than two components that do not use it. Try composition with `children` before context.

## Tests

- Saving persists across a remount; Cancel discards the draft.
- Capturing a key rebinds it, and a conflict shows the error.
- With reduced motion `'system'`, a mocked `matchMedia` change flips the resolved value.
- Calibration with fake taps 20 ms late sets the offset to about 20.

## PR questions

1. Which components re-render when one setting changes, and how did you confirm it?
2. Why does game state stay out of context?
3. Why does the form edit a draft instead of writing to context on every keystroke?

