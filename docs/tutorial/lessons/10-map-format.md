# Lesson 10: Map Format, Import and Export

Branch: `lesson/10-map-format`

Goal: maps become files a player can save, share, and load, validated by one schema that also produces the TypeScript type.

Read first: Zod docs (Basic usage, Type inference, Error handling). React Hook Form docs (Get Started, `useForm`, `register`, Schema Validation). MDN: `SubtleCrypto.digest`, `Blob`, `URL.createObjectURL`.

## Build

1. `src/maps/schema.ts`, a Zod schema for map format version 1:
   - `version`, `title`, `artist`, `mapper`, `difficulty`, `seed`.
   - `audio`: `sha256`, `durationMs`. No audio data and no file path.
   - `offsetMs`, `beats` (ascending milliseconds), `sections`.
   - Custom checks: beats ascending, sections inside the beat range, beats inside the song duration.
   - The `MapFile` type comes from `z.infer`. Delete any hand-written type it replaces.
2. `hashAudio(file)` using `crypto.subtle.digest('SHA-256', ...)`, returning hex.
3. Map details form in the editor, on React Hook Form with the Zod resolver:
   - Title, artist, mapper, and difficulty, with title and artist prefilled from the file name so the player does not type what the app can guess.
   - Native inputs use `register`. Use `Controller` only where you must, and say where.
   - Keep the accessibility behavior from lesson 8: `aria-invalid`, `aria-describedby`, and focus to the first error.
4. Export downloads `<title>.map.json`.
5. Import:
   - A file input reads a map file and runs `safeParse`. Errors are listed in plain language with the path to each problem.
   - When playing, the player's audio file is hashed and must match the map. A mismatch says so and offers to play anyway with the map's timing.
6. Starter maps for the bundled CC tracks live in `public/maps/` and pass the schema. Put the shared fixtures in `fixtures/maps/` at the repo root; the Go lessons reuse them.

## Tests

- Valid fixtures parse; each invalid fixture fails with the expected path.
- Export then import round-trips to an equal object.
- A hash mismatch shows the message.
- The details form shows field errors and focuses the first one.

## PR questions

1. How many re-renders does typing a title cause, and how does that compare with the lesson 8 form?
2. Why does the map store a hash of the audio instead of the audio or a file name?
3. What does `z.infer` replace, and what bug class does that remove?

