# Starter Tracks

Linewave ships with a small set of songs so the game plays on first load. The set covers dubstep, drum and bass, and electro house, two tracks each. Every track is CC0 or CC BY. Player audio never enters the repo; these files are the only music it holds.

## License rule

Only CC0, CC BY 4.0, or CC BY 3.0. No NC, ND, or SA licenses, and no "royalty free" or site-specific licenses (Pixabay, YouTube Audio Library). CC 4.0 counts syncing music to moving images as an adaptation, and a rhythm game does exactly that, so ND and SA are out. NC is out so the license never depends on how the game is used.

OpenGameArt licenses are set by the uploader. Before a track is committed, re-read its page, and save the page's license and credit text into `CREDITS.md`.

## The set

All six come from OpenGameArt. BPM is listed where the artist states it; the others get beat-mapped in the editor. Lengths marked "~" are estimates, so confirm them in a player.

| Genre | Title | Artist | License | Length | BPM | Page |
|---|---|---|---|---|---|---|
| Dubstep | Noizy | Of Far Different Nature | CC BY 4.0 | ~2:18 | unstated | https://opengameart.org/content/noizy |
| Dubstep | Android Assault | Umplix | CC0 | 2:28 | unstated | https://opengameart.org/content/android-assault |
| Drum and bass | Another Step Back | Of Far Different Nature | CC BY 4.0 | ~3:03 | unstated | https://opengameart.org/content/another-step-back |
| Drum and bass | Final Hour | isaiah658 | CC0 | ~2:17 | unstated | https://opengameart.org/content/final-hour |
| Electro house | Flying Temple | Tricks & Traps | CC0 | ~2:17 | 128 | https://opengameart.org/content/free-rhythm-game-music-pack-1 |
| Electro house | Electric Dream | Of Far Different Nature | CC BY 4.0 | ~3:54 | unstated | https://opengameart.org/content/electric-dream |

Alternates if one sounds wrong in play: Lost Utopia (Tricks & Traps, CC0, 127 BPM) and Ascend (Tricks & Traps, CC0, 130 BPM) from the same rhythm pack, and Eastern Arctic Dubstep (VishwaJai, CC0, 5:15).

Credit lines for the CC BY tracks follow the artist's site:

- "Noizy" by Of Far Different Nature (https://fardifferent.carrd.co), CC BY 4.0
- "Another Step Back" by Of Far Different Nature (https://fardifferent.carrd.co), CC BY 4.0
- "Electric Dream" by Of Far Different Nature (https://fardifferent.carrd.co), CC BY 4.0

## How they ship

- Files go in `public/tracks/` as OGG or MP3. Some tracks are offered only as WAV (about 24 to 40 MB each); convert those to OGG before committing so the repo stays small.
- `public/tracks/tracks.json` lists each track's title, artist, genre, license, credit line, file, BPM, and offset. Lesson 6 reads it so starter tracks play without typing a BPM.
- `public/maps/` holds one map per track (lesson 10).
- `CREDITS.md` at the repo root and the about page (lesson 14) carry every credit line and license.
