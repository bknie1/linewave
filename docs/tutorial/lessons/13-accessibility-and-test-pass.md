# Lesson 13: Accessibility and Test Pass

Branch: `lesson/13-a11y`

Goal: WCAG 2.2 AA for the shell, editor, and hub, the Game Accessibility Guidelines basics for play, and the test gaps closed.

Read first: W3C "What's New in WCAG 2.2". Game Accessibility Guidelines, Basic and Intermediate. axe-core API docs (`axe.run`). Testing Library: Which query should I use?

## Build

1. An `expectNoAxeViolations(container)` helper that runs `axe-core` directly, called in a test for every non-play screen in both themes. Fix everything it finds.
2. WCAG 2.2 items, each checked and noted in the PR:
   - 2.4.11 Focus Not Obscured: sticky headers never hide the focused element.
   - 2.5.7 Dragging Movements: every drag in the editor has a keyboard or click alternative.
   - 2.5.8 Target Size: controls are at least 24 by 24 CSS pixels or meet the spacing exception.
   - 3.2.6 Consistent Help: the help link sits in the same place on every screen.
   - 3.3.7 and 3.3.8: note where they apply or why they do not.
3. Game Accessibility Guidelines items, each checked:
   - Every input is remappable.
   - The game is playable with the sound off, through the beat cue.
   - Relaxed timing widens the windows.
   - Pause works at any time.
   - Reduced motion removes shake and flashing.
   - Pieces are distinguishable without color.
   - Nothing flashes more than three times a second.
4. Announcements: a polite live region for combo milestones and line clears, throttled so it never floods, and a full summary at the end of a song.
5. Manual pass, written up in the PR: keyboard only through menu, library, editor, hub, and settings; then the same with NVDA or Narrator; then one song with the sound off.
6. Test gaps: run coverage, list the uncovered branches that matter, and cover them. Do not chase a percentage.
7. Component library audit, written up in `docs/library-notes.md`:
   - What Radix handled for you in each shadcn/ui component you used (focus, keyboard, ARIA).
   - What you still had to fix: contrast of your theme variables in both themes, target sizes, visible focus, labels.
   - One component where you changed the generated source, and why.
   - When you would pick a headless library (Radix, React Aria, Base UI), a styled one (MUI, Mantine), or your own.

## PR questions

1. What did axe miss that the manual pass caught?
2. Which parts of play cannot be made screen-reader accessible, and how do you state that honestly on the about page?
3. Which test would you show an interviewer as your best example, and why?
4. Does using an accessible component library make an app accessible? Answer from what your audit found.

