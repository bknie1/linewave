# Lesson 9: Map Editor, Timeline

Branch: `lesson/09-editor`

Goal: the editor where a player turns their own song into a playable map.

Read first: react.dev Learn, Manipulating the DOM with Refs and Removing Effect Dependencies. MDN: Canvas API tutorial (basic drawing), `ResizeObserver`. Background on beat detection: any short article on onset detection with an energy envelope.

## Build

1. Load a song with `AudioDrop` from lesson 6.
2. Waveform
   - A pure function turns decoded samples into peak values per pixel column, at a zoom level.
   - A `<canvas>` draws them. Drawing happens in an effect keyed on peaks, zoom, scroll, and size, which is a legitimate effect because the canvas is outside React.
   - A `ResizeObserver` with cleanup keeps the canvas sharp when the layout changes.
3. Playhead: moves during playback without re-rendering the whole editor every frame. Pick a technique and defend it in the PR.
4. Beat markers
   - Rendered as focusable buttons over the canvas, so they work from the keyboard.
   - Arrow keys nudge the selected beat by 10 ms, Shift with an arrow nudges by 1 ms, Delete removes it.
   - The T key adds a beat at the playhead during playback (tap mapping).
   - If you add mouse dragging, keyboard nudging is its alternative.
5. Auto-detect
   - A pure function finds onsets: frame energy, a moving average, and peaks above it.
   - A second pure function estimates BPM from the gaps between onsets and snaps beats to a grid.
   - The result fills the beat list as a starting point the player edits.
6. Sections: a list of `{ startBeat, gravity }` entries, edited in a small table.
7. Undo and redo
   - A generic `withHistory(reducer)` that wraps the editor reducer in `{ past, present, future }`.
   - Ctrl+Z and Ctrl+Shift+Z, plus buttons.
8. Preview plays the map in a small embedded game from the current beat.

## Tests

- Peaks from a synthetic sine wave have the expected shape.
- Onset detection on a synthetic click track finds every click within 10 ms; BPM estimation returns the click rate.
- `withHistory` undo and redo, including that a new edit clears the redo stack.
- Keyboard nudging and deleting beats.

## PR questions

1. Which of your effects synchronize with an outside system, and which did you remove while building?
2. How does the playhead move without re-rendering the editor every frame?
3. Why is `withHistory` a function that wraps a reducer instead of logic inside the component?

