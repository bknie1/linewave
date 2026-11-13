# Lesson 5: Game Loop

Branch: `lesson/05-loop`

Goal: real-time gravity, done with the effect patterns React actually wants.

Read first (react.dev, Learn): Synchronizing with Effects, You Might Not Need an Effect, Lifecycle of Reactive Effects, Separating Events from Effects. Reference: `useRef`. MDN: `requestAnimationFrame`, Page Visibility API.

## Build

1. `useAnimationFrame(callback, running)`
   - Runs `requestAnimationFrame` while `running` is true and cancels it in cleanup.
   - Keeps the latest `callback` in a ref, so the effect does not restart every render.
   - Passes elapsed milliseconds to the callback.
2. Gravity: accumulate elapsed time in a ref and dispatch `tick` each time it passes the gravity interval. Speed up as lines are cleared.
3. Pause and resume with a button and a key. While paused, the board is covered by a pause panel, and focus moves to its Resume button.
4. Auto-pause when the tab is hidden, through a `visibilitychange` listener with cleanup.
5. Remove the Step button, or keep it behind a debug flag.
6. Confirm StrictMode's double mount causes no double speed or stuck loop. Write what you saw in the PR.

## Tests

- With fake timers and a faked `requestAnimationFrame`, the piece falls one row per interval.
- Pausing stops gravity; resuming continues it.
- Unmounting cancels the frame. Check with a spy.
- A hidden-tab event pauses the game.

## PR questions

1. Why does the callback live in a ref instead of in the effect's dependencies?
2. What would happen without the cleanup function under StrictMode?
3. Which of your effects synchronize with an outside system, and what is that system for each?

