# Lesson 6: Audio and the Beat

Branch: `lesson/06-audio`

Goal: the game plays a song the player supplies, and gravity and scoring follow its beat.

Read first: MDN Web Audio API (Using the Web Audio API, `AudioContext.decodeAudioData`, `AudioContext.currentTime`). react.dev Reference: `useSyncExternalStore`. react.dev Learn: Reusing Logic with Custom Hooks.

## Build

1. `AudioDrop`
   - A labelled `<input type="file" accept="audio/*">` is the primary control.
   - Drag and drop is an enhancement on top. A player who cannot drag loses nothing (WCAG 2.5.7).
   - Decoding shows a loading state, and a bad file shows a clear error.
   - The file never leaves the browser.
2. `AudioEngine` in `src/audio/`, a plain TypeScript class or module with no React:
   - Owns one `AudioContext`, created on the first user gesture.
   - `load(file)`, `play()`, `pause()`, `seek(ms)`.
   - `subscribe(listener)` and `getSnapshot()` for `{ playing, timeMs }`. It updates the snapshot once per frame while playing and returns the same object between updates.
3. `useAudioClock()` reads the engine through `useSyncExternalStore`.
4. Hide the engine behind an interface, so tests use a fake clock. jsdom has no Web Audio.
5. Beats for now: BPM and offset as controlled number inputs. A pure function turns them into beat times in milliseconds.
6. Rhythm rules, in the engine from lesson 3:
   - Gravity ticks on beats instead of a fixed interval.
   - Each move or rotate carries a judgment (`'perfect' | 'good' | 'miss'`) from comparing its time with the nearest beat.
   - The combo grows on Perfect and Good and resets on Miss. Line clears on the beat get a bonus.
7. A visual beat cue (a pulse on the board frame) so the game is playable with the sound off.
8. Starter tracks: the bundled songs in `public/tracks/` (see `docs/tutorial/starter-tracks.md`) load through `fetch` into the same engine. Their BPM and offset come from `public/tracks/tracks.json`, so they play without typing anything.

## Tests

- Beat-time generation from BPM and offset, and nearest-beat judgment at the window edges.
- With the fake clock, gravity ticks on each beat.
- The combo grows and resets as judgments come in.
- `AudioDrop` accepts a file through the input and shows the error state for a non-audio file.

## PR questions

1. Why is `useSyncExternalStore` the right hook for the audio clock, and what goes wrong if `getSnapshot` returns a new object every call?
2. Why does the `AudioContext` wait for a user gesture?
3. What did hiding the engine behind an interface buy you in tests?

