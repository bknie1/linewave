# Starter Tracks

Linewave ships with six songs so the game plays on first load: two each of dubstep, drum and bass, and electro house. Every track is CC0 or CC BY. Player audio never enters the repo; these files are the only music it holds.

## License rule

Only CC0, CC BY 4.0, or CC BY 3.0. No NC, ND, or SA licenses, and no "royalty free" or site-specific licenses (Pixabay, YouTube Audio Library). CC 4.0 counts syncing music to moving images as an adaptation, and a rhythm game does exactly that, so ND and SA are out. NC is out so the license never depends on how the game is used.

OpenGameArt licenses are set by the uploader. Any track added later gets its page re-read and its license and credit recorded in `CREDITS.md` before it is committed.

## The set

The files are in `public/tracks/`, and `public/tracks/tracks.json` describes them. Credits and licenses are in [CREDITS.md](../../CREDITS.md).

| Genre | Title | Artist | License | Length | BPM |
|---|---|---|---|---|---|
| Dubstep | Noizy | Of Far Different Nature | CC BY 4.0 | 1:09 | 140 (estimated) |
| Dubstep | Android Assault | Umplix | CC0 | 2:28 | 146.5 (estimated) |
| Drum and bass | Another Step Back | Of Far Different Nature | CC BY 4.0 | 3:03 | 169.5 (estimated) |
| Drum and bass | Final Hour | isaiah658 | CC0 | 2:17 | 169.5 (estimated) |
| Electro house | Flying Temple | Tricks & Traps | CC0 | 2:17 | 128 (stated) |
| Electro house | Electric Dream | Of Far Different Nature | CC BY 4.0 | 3:54 | 127 (estimated) |

Only Flying Temple states its BPM. The others were estimated from an onset envelope with autocorrelation. That method gave 128 for Flying Temple, which matches its stated BPM. The estimate cannot tell a tempo from its half or double, so the dubstep and drum and bass values were chosen from each genre's usual range. Treat the estimates and offsets as starting points: lesson 6 plays them as they are, and the lesson 9 editor is where you confirm them by ear. They also make a good check for your own beat detector in lesson 9.

Alternates if one plays badly: Lost Utopia (127 BPM) and Ascend (130 BPM) from the same Tricks & Traps CC0 pack as Flying Temple, and Eastern Arctic Dubstep (VishwaJai, CC0, 5:15).

## `tracks.json`

Each entry has `id`, `title`, `artist`, `genre`, `file`, `durationMs`, `bpm`, `offsetMs`, `bpmSource` (`"stated"` or `"estimated"`), `license`, `credit`, and `source`. Lesson 6 reads it so starter tracks play without typing a BPM. Lesson 10 adds one map per track in `public/maps/`. Lesson 14 shows every `credit` line on the about page.
