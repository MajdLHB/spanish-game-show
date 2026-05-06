# Build Report

A running log of what was built, in plain English. Each entry is dated and self-contained. Newest on top.

---

## 2026-05-06 (later) — Full redesign: EL GRAN SHOW broadcast edition

### Goal
Redo the project as a **high-end, competitive TV-game-show** for a Spanish class. The earlier "¡Adivina la Profesion!" was a working but basic prototype; this iteration is a complete production-style rewrite: fixed 16:9 stage, persistent broadcast scoreboard, presenter-driven step machine, hardcoded intersecting crossword (`SUPERMERCADO`, `JOYERÍA`, `PROFESORA`, `INGENIERO`), and overlays for hint / reveal / transition / video / live-guest / finale.

### Files touched

| File | Action |
|---|---|
| `index.html` | **Replaced** completely (~880 lines). New 16:9 stage architecture, step-machine flow, broadcast styling. |
| `README.md`  | **Replaced** to document the new app — flow, customization hooks, host controls, tech notes. |
| `REPORT.md`  | This entry appended on top of the previous one. |

The previous v1 of the app remains in git history if needed.

### What's new vs. the previous version

- **Fixed 16:9 stage**: a `1920×1080` element is rendered internally and JS scales it via `transform: scale(min(w/1920, h/1080))` on every resize. Layout never warps on different projectors or windows.
- **Persistent broadcast scoreboard**: top-of-stage strip with Team A (cyan) / Team B (pink) blocks, each with two player-name pill placeholders, large neon score numbers (~6.5rem), gold "VS" divider, and a `bump` keyframe animation that fires on score change.
- **Linear presenter-controlled step machine**: 22 steps total — `preshow → countdown → intro_logo` → for each of 4 words: `highlight → hint → reveal → transition → feature` → `finale`. A floating "NEXT ▶" button (with a SPACE keycap badge) and the Spacebar both advance. The reveal step blocks both — you must click +1 POINT TEAM A/B to award a point and advance. Auto-advance steps (countdown, intro logo, logo flash) handle their own timing.
- **Hardcoded crossword**: 13×12 grid (only 33 cells exist; the rest are `visibility:hidden` placeholders). Verified intersections:
  - `SUPERMERCADO` (row 3) ∩ `PROFESORA` (col 3) at letter **P**
  - `SUPERMERCADO` (row 3) ∩ `INGENIERO` (col 4) at letter **E**
  - `JOYERÍA` (row 7) ∩ `PROFESORA` (col 3) at letter **E**
  - `JOYERÍA` (row 7) ∩ `INGENIERO` (col 4) at letter **R**
  - The `Í` in `JOYERÍA` is correctly stored and rendered (`Array.from()` is used so the accented character isn't broken by `String.split("")`).
- **Differentiated hints per word**:
  - W1: text overlay with a long descriptive hint.
  - W2: 3-emoji card with staggered pop-in animation.
  - W3 / W4: no overlay — just a small bottom-of-board cue badge that says "🎤 EL PRESENTADOR INTRODUCE LA PALABRA".
- **Differentiated features per word**:
  - W1 / W2: full-screen YouTube embed with autoplay parameter appended.
  - W3 / W4: golden "★ INVITADO EN VIVO ★" guest title card with the word + a placeholder presenter name.
  - Both have a "↩ BACK TO BOARD" button, which is just an alias for advancing.
- **Reveal screen**: full-screen gradient word with zoom-in + animated gradient-shift, sparkle particles spawned over the lower half, and the two large color-coded "Point Team A / Point Team B" buttons. Behind the overlay, the corresponding crossword cells fill in with a 3D card-flip animation, staggered ~70 ms each, so the letters are still revealed when overlays close.
- **Transition step**: short logo flash overlay (1.5 s, with sound effect placeholder), auto-advance.
- **Finale**: hides scoreboard + crossword, shows trophy, big gradient "WINNER: TEAM X" (or "EMPATE — TIE!" on a tie), final score breakdown, theme song fade-out, and continuous CSS confetti rain. "PLAY AGAIN" reloads.
- **Visual polish**: animated background grid, subtle TV scanlines via `repeating-linear-gradient` blended over everything, neon glow shadows, gradient gradients (background-position keyframe sweeps), drop-shadows on logo / word reveals.
- **Customization hooks**: every swap-in value (audio file paths, video URLs, guest names, hint content, player names, studio image URL) is clearly marked with `PLACEHOLDER` comments or obvious replace tokens.

### Behavior details worth noting

- Theme song begins on countdown step (after first user click, satisfying browser autoplay policy).
- Audio elements are tolerant of missing files — `try/catch` around `play()` plus `.catch(() => {})`.
- Spacebar handler ignores key presses while focus is in an `INPUT` or `TEXTAREA` (defensive, even though no inputs exist in normal flow).
- `awardPoint()` clears `videoIframe.src` defensively so a previous video doesn't keep playing in the background.
- Step indicator in the bottom-right shows live progress (e.g. `STEP 7 / 22`).

### Things deliberately not done

- No `localStorage` persistence — refreshing resets the show. (Spec says reload-restart via "PLAY AGAIN" is acceptable.)
- No way to swap the 4 words without also re-deriving the grid coordinates. Documented in README as a manual operation.
- No "Back" button in the step machine — presenter-controlled forward-only flow keeps the broadcast vibe and avoids accidental rewinds during a live show.
- No real sound effects bundled — only `<audio>` tag placeholders. Browsers will silently no-op when the source files don't exist.

### How it was tested

Static-built only (no dev server). Logic was self-checked by tracing through the step machine and verifying:
- All 22 steps reachable in order.
- Reveal blocks Spacebar / Next; Point A/B buttons release it.
- Crossword intersections all match (each shared cell's letter agrees in both words).
- 16:9 scale math: `min(w/1920, h/1080)` keeps aspect ratio on any window.

The user should open `index.html` in a browser to verify visually before showtime.

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
