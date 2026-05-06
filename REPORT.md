# Build Report

A running log of what was built, in plain English. Each entry is dated and self-contained.

---

## 2026-05-06 — Initial build

### Goal
Stand up a single-file, browser-based TV-game-show app for a Spanish class (theme: professions / jobs), with start screen → animated logo → game board → hint modal → reveal celebration → broadcast video → loop back, plus a confetti winner screen. Sleek dark-mode neon TV-show aesthetic.

### What was created

| File | Purpose |
|---|---|
| `index.html` | The entire app — HTML, CSS, JS in one self-contained file. |
| `README.md`  | What the project is, how to play, and where to swap in your own words / sound / video. |
| `REPORT.md`  | This file — a running build log. |

### What `index.html` contains

A single HTML document holding **6 screens** that swap in/out via a tiny screen-router:

1. **Start screen** — neon gradient title, animated pulsing glow, "Start Game" button.
2. **Logo / intro** — three counter-rotating neon rings around a gradient title core. Plays the placeholder intro audio and auto-advances after 3 s.
3. **Board** — scoreboard at top (Team A on cyan, Team B on pink, with ± controls and a gold "VS" between them), and a crossword-style grid below with 5 numbered word rows. Each row is `[number button] [empty letter cells]`. Cells flip in with a 3D-pop animation when revealed.
4. **Hint modal** — opens when a number is clicked. For words 1–4 it shows a "Show Hint" button that animates in 3 emojis; for word **5** (final round) it swaps in a different panel with a "Reveal Half Letters" button that randomly reveals ⌈n/2⌉ of the word's letters directly on the board. Either flow ends with a "Reveal Answer" button.
5. **Reveal screen** — the word fills the screen with a multi-color gradient + zoom-in animation; gold/cyan/pink/purple sparkle particles spawn. Has a "Play Video" button.
6. **Video screen** — a 16:9 iframe containing the placeholder YouTube embed. "Back to Board" button stops the video, briefly replays the logo screen (~1.8 s), and returns to the updated board.
7. **Winner screen** — full-screen confetti layer (initial burst + continuous trickle), trophy bounce, gradient banner, gold-glowing input pre-filled with the current leader, "Play Again" reload button.

### Theme / styling

CSS variables drive a unified neon palette: `--neon-blue`, `--neon-cyan`, `--neon-purple`, `--neon-pink`, `--neon-gold`. Glow effects use multi-layer `box-shadow` and `text-shadow` plus `drop-shadow` filters. A subtle animated grid sits behind everything as a `body::before` layer. Buttons share a `.neon-btn` class with cyan→purple hover transitions and a `.gold` / `.pink` accent variant for special actions (Finish Game, Final Round).

Animations used: fade-in screen swap, gradient title pulse, ring spin (3 different speeds + reverse), 3D card pop on letter reveal, gradient shift on the reveal word, sparkle float-up, confetti fall with rotation, trophy bounce, emoji slide-up.

### Customization hooks (placeholders)

Marked clearly so they're easy to find:
- `WORDS` array at the top of the script — change the 5 Spanish words and emoji hints.
- `<audio id="intro-sound">` `<source src="REPLACE_WITH_YOUR_INTRO_SOUND.mp3">` — drop in your epic intro mp3.
- `PLACEHOLDER_VIDEO_URL` — replace with your YouTube embed URL.

### Behavior details worth noting

- **Score adjustments** never go below 0.
- **Reveal state** persists across screen transitions — when you come back from the video, the board correctly shows previously revealed words and gold-tints their number buttons.
- **Final-round half-reveal** picks indices uniformly at random from the *not-yet-revealed* set, so calling it twice will keep adding letters until everything is shown rather than re-flipping the same ones.
- **Esc** closes any open modal.
- Backdrop click on the hint modal also closes it.

### Things deliberately not done (out of scope for v1)

- No `localStorage` — refreshing resets state. (Listed in the README as a possible extension.)
- No real crossword intersections — the grid is row-based, one word per row with a number marker. Visually it reads as a crossword without the pain of designing intersections that fit any 5 words you swap in.
- No sound effects beyond the intro.
- No backend, no build step, no dependencies.

### How it was tested

Static-built only — no dev server stood up. Logic was self-checked by reading through each event handler and confirming state transitions match the requested flow. The user should open `index.html` in a browser to verify visually before the class.

---

## How this report works going forward

Every meaningful change to the project should append a new dated section here describing **what changed and why**. New entries go on top of the most recent one (newest-first), under their own `## YYYY-MM-DD — short title` heading. Keep entries concrete: list files touched, behavior changes, and anything intentionally left undone.
