# Build Report

A running log of what was built, in plain English. Each entry is dated and self-contained. Newest on top.

---

## 2026-05-06 (later still) — Broadcast Edition: bright editorial theme + true intersecting crossword + Coming Up Next segue

### Goal
Three structural changes from the previous "EL GRAN SHOW" build, all aimed at making the app look and behave like a professional TV broadcast package rather than a web/arcade game:

1. **Drop the dark/neon aesthetic entirely** and rebuild as a bright, editorial, daytime-broadcast look (cream paper, navy + coral, Playfair Display + Inter, lower-third chyrons, soft shadows). No glows, no scanlines, no gradient sweeps. No default HTML buttons anywhere.
2. **Make the crossword physically intersect** — the previous version's grid had gap-separated rounded tiles, which read as four floating word strips rather than a real crossword. Tighten the grid so cells touch and share their gridlines at the four intersection points.
3. **Replace the "Game Over" finale** with a graceful winner-then-segue sequence ending on a clean "Coming Up Next" bumper, so the segment ends smoothly into other class activities instead of feeling like the end of a video game.

### Files touched

| File | Action |
|---|---|
| `index.html` | Replaced (~1,100 lines). Same JS architecture (step machine, scaled stage), but completely new CSS theme, new winner + segue overlays, tighter crossword. |
| `README.md`  | Rewritten to describe the broadcast edition, the segue, the new look, and customization hooks (added `up-next-title` placeholder + `guest.initial` field). |
| `REPORT.md`  | This entry appended on top. |

### What's new in this iteration

#### Aesthetic — full editorial broadcast look
- **Color system**: paper white / cream-`#faf6ed`, ink-`#15233a`, editorial navy-`#14365b`, warm coral-`#d34b35`, gold-`#c9a04a`, mint-`#4eb89c`. Soft drop shadows (`0 8px 28px rgba(21,35,58,.10)`); zero glow / blur effects.
- **Typography**: imported `Playfair Display` (serif headlines, words, score numbers) and `Inter` (UI / chyron labels, buttons) via Google Fonts `@import`. Falls back to Georgia / system sans when offline.
- **Lower-third scoreboard** at the top: white card, 4-px deep-navy bottom border, soft drop shadow, color stripes by team (navy / coral), all-caps Inter labels with wide letter-spacing, big serif `5.5rem` scores, center "EN VIVO · EL GRAN SHOW · Spanish Class · Crossword Segment" identifier.
- **Pre-show**: cream backdrop, "ON AIR" red pill with blinking dot, gold rule pair, big serif headline, custom "Begin Broadcast" navy block button with a play-triangle.
- **Countdown** is in serif numbers inside a thin gold ring.
- **Hint card**: white card with a dark-navy chyron header bar (`PISTA · HINT` + word number), soft shadow, generous white interior.
- **Reveal**: small gold "La Respuesta · The Answer" tag, giant serif word, gold underline that animates in via `scaleX`, and two custom award buttons styled as broadcast cards (`+1` serif numeral + 2-line label "Award point / Team A").
- **Feature**: video frame is a white card padding around a 16:9 iframe with a small red `EN VIVO` chyron pill above; guest card has a navy chyron header, a circular gold-ringed photo placeholder showing the guest's initial, name in serif, profession label in gold tracked caps, and the word in editorial blue serif.
- **Confetti** is paper-style: small 14×22-px rectangles in navy / coral / gold / mint / white / muted ink, slower fall (3.5–7 s), gentler opacity (.55–.9). No neon colors, no spinning sparkles.
- **No default HTML buttons** — every interactive element is custom-styled (start, next, point, back, continue, restart).

#### Crossword — true physical intersections
- `.crossword` now uses `gap: 0` (was `4px`).
- Each `.cell.exists` has `outline: 1.5px solid var(--ink); outline-offset: -.75px;`. Adjacent cells' outlines visually merge into one shared gridline. Empty cells have `background: transparent` and no outline so they don't render at all.
- Intersection cells are still rendered exactly once in the DOM (the underlying `gridMap` keys by `"r,c"`), but now with the tight grid the player can clearly see the four words physically share four cells: P/P at (3,3), E/E at (3,4), E/E at (7,3), R/R at (7,4).
- `border-radius` on cells removed — they're now sharp newspaper-style squares, not rounded tiles.
- Number badges moved to coral and the small Inter font; they now feel like newspaper crossword numbers, not arcade UI.
- Highlighted state is gold tint (`var(--gold-2)`) with a thicker gold outline at `z-index: 2`, so the active word reads clearly without losing the grid.

#### Winner → Segue (Coming Up Next)
- Old `finale` step removed. Replaced with two consecutive steps:
  - `winner` — full-stage editorial result card. Small "Resultado Final · Final Result" tag, "Ganadores · Winners" eyebrow, big serif team name in their team color (navy/coral) or "Empate · Tie" centered, gold rule, paper-white "final scores" card showing both team totals, and a "Continue →" button. Light continuous paper-confetti.
  - `segue` — clean broadcast bumper. Cream background, "Coming Up Next" gold tag with twin rules, big "EL GRAN SHOW" centered, gold rule, "Más a continuación · Up Next: Conversation Practice" (placeholder text the user can edit), "Gracias por jugar · Thank you for playing", and a tiny `↺ Restart Show` pill bottom-right. The floating Next button hides when entering segue so the bumper is clean.
- **Graceful cross-fade**: clicking "Continue" adds a `.fading` class to the winner overlay (`opacity: 0` over 0.9 s), then advances to segue, which has its own `segueFadeIn` keyframe animation (~1.2 s easing in from `opacity: 0` and a subtle `scale(.985 → 1)`). The result reads as a real broadcast bumper rather than an abrupt screen swap.
- A `state.isFading` flag prevents the user from double-pressing through the cross-fade.

#### Other improvements
- Step count: now **25** total (was 23). Shown as `Step n / 25` in the indicator.
- `WORDS[].feature.initial` added for guest features so the circular photo placeholder shows a serif letter (`G`, `R`, etc.) rather than a generic icon.
- Bilingual labeling throughout (Spanish primary + English subtitle) — "Pista · Hint", "La Respuesta · The Answer", "Invitado en Vivo · Live Studio Guest", "Ganadores · Winners", "Más a continuación · Up Next" — appropriate for a Spanish class.
- Outer letterbox (window background outside the stage) is now neutral `#0a0a0a` instead of decorative — matches a real broadcast safe-area.
- The "ON AIR" pill in the pre-show has a blinking white dot, which is the real-world TV cue for "we are recording / live".

### Things deliberately not done

- The four crossword words still cannot be swapped without re-deriving grid coordinates. Calling that out in the README is enough — auto-layout for arbitrary intersecting word sets is a much bigger feature.
- No `localStorage` for restart-resilience — refresh still resets state. The "Restart Show" pill on the segue and a hard reload are the two restart paths.
- No fancy "Up Next" carousel of class activities. The placeholder is one line; if the user wants more, they'll edit the HTML.

### How it was tested

Static-built only (no dev server). Verified by tracing the step machine through all 25 states and checking:
- Crossword intersections render as a single DOM cell (gridMap dedup is unchanged from prior version).
- `Array.from()` correctly preserves the accented `Í` in `JOYERÍA` (string is 7 grapheme clusters; `Í` is one of them).
- Winner-to-segue cross-fade timing: 0.9 s `opacity → 0` followed by segue's 1.2 s fade-in feels deliberate, not jarring. `state.isFading` prevents double-clicks during the transition.
- Audio play() failures (placeholder mp3 files) are silently caught.
- Stage scaling math unchanged: `min(w/1920, h/1080)`.

The user should open `index.html` in a browser to confirm the look before showtime — Google Fonts will load, falling back to Georgia / system sans if offline.

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
