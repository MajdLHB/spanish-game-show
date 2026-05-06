# EL GRAN SHOW — Spanish Class Game Show

A single-file, broadcast-quality TV game show for a Spanish class. Two student teams compete to guess **4 intersecting Spanish words** on a crossword grid (`SUPERMERCADO`, `JOYERÍA`, `PROFESORA`, `INGENIERO`). The presenter advances the show one beat at a time using a floating "Next" button or the spacebar.

The whole game is one self-contained `index.html` file (HTML + CSS + JS). No build step, no server, no dependencies.

---

## Highlights

- **Fixed 16:9 stage (1920×1080)** that auto-scales to fit any window/projector — the layout never breaks regardless of resolution.
- **Persistent broadcast scoreboard** at the top with Team A / Team B labels, two player-name placeholders per team, and big neon score numbers that bump-animate when a point is awarded.
- **Hardcoded crossword** with verified intersections of all 4 words at letters P, E, E, R.
- **Linear step machine** — 22 steps total, each advanced manually by the presenter (Next button or Spacebar) so you have full pacing control. A small step indicator in the corner shows where you are.
- **Sleek dark-mode neon aesthetic** — neon blue / purple / pink / gold, animated background grid, subtle TV scanlines, gradient logo, glow effects, and smooth transitions throughout.

---

## The flow (22 steps, presenter-paced)

1. **Pre-Show** — studio backdrop image and a giant pulsing "▶ START SHOW" button.
2. **Countdown** — full-screen `3… 2… 1…` with the theme song kicking in.
3. **Intro Logo** — animated "EL GRAN SHOW" with a zoom + gradient sweep, then auto-advances.
4. **Per word, 5 sub-steps × 4 words = 20 steps:**
   - **Highlight** — the current word's tiles pulse on the crossword grid.
   - **Hint**:
     - Word 1 (`SUPERMERCADO`) — text hint overlay.
     - Word 2 (`JOYERÍA`) — three big animated emojis (💍 💎 ⌚).
     - Words 3 & 4 (`PROFESORA`, `INGENIERO`) — no on-screen hint, just a small "🎤 EL PRESENTADOR INTRODUCE LA PALABRA" cue at the bottom signaling the host to introduce it live.
   - **Reveal** — full-screen massive gradient word with sparkles + sound, plus two big buttons: `+1 POINT TEAM A` / `+1 POINT TEAM B`. Clicking one updates the global score and advances. The word's letters also fill into the board behind so it remains revealed.
   - **Transition** — short logo flash with sound effect, then auto-advances.
   - **Feature**:
     - Words 1 & 2 — full-screen YouTube broadcast iframe.
     - Words 3 & 4 — "★ INVITADO EN VIVO ★" guest title card with the word + a placeholder name.
     - Both have a "↩ BACK TO BOARD" button (or press Next / Space).
5. **Finale** — scoreboard and crossword are hidden. Full-screen results: bouncing trophy, gradient "WINNER: TEAM X" (or `EMPATE — TIE!` if scores are equal), the final score breakdown, CSS confetti rain, and a "🔄 PLAY AGAIN" button.

---

## Customizing for your class

All the easy-to-swap values are clearly marked with `// PLACEHOLDER` comments in the code.

### Player names (top of `<body>`)
Find the four `<span class="player-pill">` tags inside the `<div class="scoreboard">` block:
```html
<span class="player-pill">Player 1</span>
<span class="player-pill">Player 2</span>
…
<span class="player-pill">Player 3</span>
<span class="player-pill">Player 4</span>
```
Replace with your students' names.

### Team labels
Same scoreboard block — change the `<div class="team-label">TEAM A</div>` and `TEAM B` text if you want different team names.

### Hints, video URLs, and guest names
Top of the `<script>` block, the `WORDS` array. Each entry has a `hint` and a `feature`:
```js
{ id: 1, word: "SUPERMERCADO", direction: "horizontal", startRow: 3, startCol: 1,
  hint:    { kind: "text", value: "Aquí compras tu comida cada semana…" },
  feature: { kind: "video", url: "https://www.youtube.com/embed/dQw4w9WgXcQ" } // PLACEHOLDER — replace
},
```
- `hint.kind` can be `"text"`, `"emojis"` (array of 3), or `"none"`.
- `feature.kind` can be `"video"` (with `url`, use the YouTube `/embed/` form) or `"guest"` (with `name`).

### Audio (theme song, logo flash, reveal)
Three `<audio>` tags near the top of the stage. Drop your own files next to `index.html` and update the `src` attributes:
```html
<source src="REPLACE_WITH_THEME_SONG.mp3"        type="audio/mpeg">
<source src="REPLACE_WITH_LOGO_FLASH_SOUND.mp3"  type="audio/mpeg">
<source src="REPLACE_WITH_REVEAL_SOUND.mp3"      type="audio/mpeg">
```

### Pre-show studio image
The pre-show background is a placeholder Unsplash URL inside `.preshow-bg`. Replace the URL with your own studio image, or drop a local file next to `index.html` and reference it.

### Want to swap out the 4 words?
The grid is hardcoded for these specific intersections — if you change the words, you'll also need to re-derive `startRow` / `startCol` / `direction` so they intersect cleanly. The intersections used here are documented as an ASCII diagram inside the `WORDS` block.

---

## Host controls (presenter cheat-sheet)

| Action | How |
|---|---|
| Advance to next step | **Spacebar** or click the floating **NEXT ▶** button (bottom-right) |
| Award a point | Click **+1 POINT · TEAM A** or **+1 POINT · TEAM B** on the reveal screen (this also advances) |
| Return to board from a feature | Click **↩ BACK TO BOARD** or press Spacebar |
| Restart the whole show | Click **🔄 PLAY AGAIN** on the finale screen |
| Step indicator | Bottom-right corner shows `STEP n / 22` |

Reveal step **does not** advance via Spacebar — you must click one of the point buttons. Countdown / intro-logo / logo-flash steps auto-advance and ignore Spacebar.

---

## Tech details

- **Single file**, no dependencies, no build.
- **16:9 stage** is implemented with a `1920×1080` element transformed by `scale(min(window.width/1920, window.height/1080))`. Crisp on any monitor or projector.
- **State machine** is a flat `STEPS` array (22 entries); `state.step` is the index. Each `applyStep()` call sets up the layers/overlays for that beat.
- **Crossword** is a CSS-grid 13×12 board; non-word cells are `visibility: hidden` placeholders so the grid stays a clean rectangle.
- **Browsers**: tested mentally against Chrome / Edge / Firefox / Safari. Audio autoplay starts only after the user clicks "Start Show" (browser policy compliant).

---

## License
Use it however you want for your class. No warranty.
