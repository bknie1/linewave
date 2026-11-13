# Lesson 3: Game Engine as Pure TypeScript

Branch: `lesson/03-engine`

Goal: the whole game's rules as a pure reducer with no React in it, fully unit tested. Everything after this lesson only displays and drives it.

Read first: TypeScript handbook, Narrowing (discriminated unions) and Object Types (`readonly`). react.dev Learn: Extracting State Logic into a Reducer (the "writing reducers well" section).

## Build

In `src/game/`, with no imports from React:

1. Types
   - `Piece`: kind, rotation, and position.
   - `GameState`: board, active piece, next queue, hold, score, lines, combo, status (`'ready' | 'playing' | 'paused' | 'over'`), and RNG state.
   - `GameAction`: a discriminated union such as `{ type: 'move'; dx: -1 | 1 }`, `{ type: 'rotate'; dir: 1 | -1 }`, `{ type: 'softDrop' }`, `{ type: 'hardDrop' }`, `{ type: 'hold' }`, `{ type: 'tick' }`, `{ type: 'start'; seed: number }`.
2. Shapes and rotation for your piece set, with simple wall kicks. Document the kick rule in a comment.
3. A seeded random generator, so a seed always produces the same piece order. Maps use this later.
4. `gameReducer(state, action): GameState`
   - Moves and rotations that collide are ignored.
   - `tick` moves the piece down or locks it.
   - Locking clears full rows, scores them, spawns the next piece, and ends the game if the spawn collides.
   - Hold swaps once per piece.
   - Every action returns a new state. The input is never mutated.
5. A `switch` on `action.type` with an exhaustive check, so adding an action without handling it is a compile error.

## Tests

- Movement, rotation, and kicks against walls and filled cells.
- Locking and clearing one, two, and several rows; rows above fall correctly.
- The same seed gives the same first 20 pieces.
- Hold works once per piece.
- Game over on a blocked spawn.
- A deep-frozen input state passes through every action without throwing, which proves nothing mutates.

## PR questions

1. Why keep the engine free of React?
2. What does the exhaustive check do at compile time, and what would break without it?
3. Why does the RNG state live inside `GameState` instead of calling `Math.random()`?

