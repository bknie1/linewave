# Lesson 12: Performance

Branch: `lesson/12-performance`

Goal: a steady frame rate during play, reached by measuring first and memoizing only where the numbers say so.

Read first (react.dev, Reference): `memo`, `useMemo`, `useCallback`, `useDeferredValue`, `<Profiler>`. React Compiler docs: Introduction, Installation for Vite.

## Build

1. Install React DevTools in your browser.
2. Measure: record 10 seconds of play with the Profiler, plus the browser's Performance panel. Note renders per second, the slowest commit, and dropped frames.
3. Fix, measuring after each change:
   - Move anything that changes every frame (beat pulse, playhead, falling animation) out of React state. Use refs with CSS custom properties, or canvas.
   - `memo` on board rows or cells, if the Profiler shows they re-render without changes.
   - `useCallback` only for callbacks passed to memoized children.
   - `useDeferredValue` on the hub search box.
4. Undo any memoization that did not move the numbers.
5. On a second commit, enable React Compiler. Remove your manual `memo`, `useMemo`, and `useCallback` and measure again. Keep it on or off, with a reason.

## PR body

A table with one row per change: renders per second during play, slowest commit in ms, dropped frames over 10 seconds.

## Tests

- Existing tests still pass with the compiler on.

## PR questions

1. Which change made the biggest difference, and why that one?
2. Why does `memo` on a cell do nothing if the parent passes a new object each render?
3. After enabling the compiler, what manual optimization would you still keep?

