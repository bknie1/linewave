# Lesson 4: Playable Board, One Step at a Time

Branch: `lesson/04-playable`

Goal: wire the engine to the screen and the keyboard, with no timer yet. Gravity happens when you press a Step button.

Read first (react.dev, Learn): the whole "Adding Interactivity" chapter, plus Preserving and Resetting State. Reference: `useReducer`.

## Build

1. `Game` holds the engine with `useReducer(gameReducer, ...)`.
2. Keyboard input goes through `onKeyDown` on the focusable game area. No `useEffect`, no `window` listener yet.
   - A default key map in one constant: arrows, rotate keys, hard drop, hold.
   - Handled keys call `preventDefault` so the page does not scroll.
3. A Step button dispatches `tick`.
4. Derived during render, never stored:
   - The ghost piece (where a hard drop would land).
   - The board with the active and ghost pieces drawn onto it.
   - The next queue preview.
5. `Panel`s show Next, Hold, Score, Lines, and Combo.
6. Game over shows a `Notice` and a Restart button. Restart remounts `Game` by changing its `key`.

## Rules for this lesson

- No `useEffect`.
- No state other than the reducer, except what restart needs.

## Tests

- Arrow keys move the piece; the rendered board shows it in the new place.
- Hard drop lands where the ghost was.
- Step until a row fills, and the row clears.
- Restart after game over gives a fresh board.

## PR questions

1. Why is the ghost piece computed during render instead of stored?
2. Why does restart use `key` instead of dispatching a reset action? When would a reset action be better?
3. A handler dispatches `move` three times in a row. What does the next render show, and why is that different from calling `setX(x + 1)` three times?

