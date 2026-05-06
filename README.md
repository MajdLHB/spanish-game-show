# EL GRAN SHOW — Broadcast Edition

A single-file, **broadcast-grade** TV game show for a Spanish class. It looks like a daytime morning show — cream and white surfaces, deep editorial navy and warm coral accents, Playfair Display serif headlines, sleek lower-third chyrons. No dark mode, no neon, no arcade vibe. Two student teams compete to guess **four physically-intersecting Spanish words** on a real crossword grid (`SUPERMERCADO`, `JOYERÍA`, `PROFESORA`, `INGENIERO`). The presenter advances the broadcast one beat at a time via a small bottom-right control or the spacebar.

The whole show is one self-contained `index.html`. No build step, no server, no dependencies (Google Fonts is loaded by `@import` for the premium typography; falls back gracefully when offline).

---

## The look

- **Bright editorial palette** — cream paper `#faf6ed`, ink `#15233a`, editorial navy `#14365b`, warm coral `#d34b35`, gold `#c9a04a`. No glow effects, no neon, no scanlines.
- **Typography** — Playfair Display (serif, for headlines / words / scores) and Inter (sans, for UI / chyron labels). Clean tracking and weights.
- **Lower-third scoreboard** at the top of the stage: white card with deep-navy bottom border, color stripes by team, all-caps tracked Inter labels, big serif scores, and a center "EN VIVO · EL GRAN SHOW" identifier.
- **Newspaper-style crossword** — white cells with crisp 1.5-px navy outlines, **`gap: 0` so cells physically touch and share their outlines at intersections**. Number badges in coral at the head of each word. Highlighted (current) word tints to gold. The four words actually share four cells on the grid: the `P` in `SUPERMERCADO`/`PROFESORA`, the `E` in `SUPERMERCADO`/`INGENIERO`, the `E` in `JOYERÍA`/`PROFESORA`, and the `R` in `JOYERÍA`/`INGENIERO`.
- **Custom buttons everywhere** — pill / rectangle blocks with editorial type, soft drop shadows, no default browser styling.
- **Paper confetti & soft particles** instead of neon sparkles — small navy / coral / gold / mint / white rectangles drifting at slower speeds, like a real broadcast.

---

## The flow (25 steps, presenter-paced)

1. **Pre-show** — cream backdrop, "ON AIR" pill, gold rule, big "EL GRAN SHOW" headline, "BEGIN BROADCAST" button.
2. **Countdown** — 3 / 2 / 1 in serif inside a thin gold ring; theme song fires here.
3. **Intro logo** — animated full-stage logo card with thin gold rules above and below.
4. **Per word, 5 sub-steps × 4 words = 20 steps:**
   - **Highlight** — current word's tiles tint to gold and pulse subtly.
   - **Hint** — navy chyron header, then:
     - W1 (`SUPERMERCADO`): text hint.
     - W2 (`JOYERÍA`): three large emojis (💍 💎 ⌚).
     - W3 / W4 (`PROFESORA`, `INGENIERO`): no overlay; a small white "El presentador introduce la palabra" cue under the board signals the host to introduce the word live.
   - **Reveal** — full-stage editorial answer card: small "La Respuesta" tag, giant serif word, gold underline, two custom Award-Point buttons (one navy for Team A, one coral for Team B). Letters fill into the crossword with a card-flip animation in the background.
   - **Transition** — short logo flash (1.5 s + sound), auto-advances.
   - **Feature** —
     - W1 / W2: "Broadcast · Video" — clean white-framed YouTube iframe with a small red "EN VIVO" pill above.
     - W3 / W4: "Invitado en Vivo · Live Studio Guest" — chyron card with a circular gold-ringed photo placeholder, the guest's name, profession label, and the word.
     - Both have a "↩ Back to the Board" button.
5. **Winner** — editorial result card: "Ganadores · Winners" eyebrow, big serif team name in their team color (or "Empate · Tie"), final scores card, and a "Continue →" button.
6. **Segue ("Coming Up Next")** — when the host clicks "Continue", the winner card **gracefully fades** into a clean station bumper: thin gold rules, "EL GRAN SHOW" centered, "Más a continuación · Up Next: <activity>", "Gracias por jugar · Thank you for playing". The floating Next button hides so the bumper is clean. A small "↺ Restart Show" pill sits in the bottom-right corner if the host wants to run the show again.

The Segue is **not** a "Game Over" screen — it is a graceful broadcast bumper that lets you transition smoothly into your next class activity.

---

## What changed in this version

- **Aesthetic flipped from dark-neon to bright editorial broadcast** — every glow effect, scanline, gradient sweep, and neon color was removed. Colors and shadows are now soft and editorial; type is serif + sans on cream paper.
- **Crossword intersections are physically shared** — `gap: 0` plus 1.5-px outlines on each cell, with one DOM cell per `(row, col)` regardless of how many words cross there. Adjacent cells' outlines overlap, producing one continuous grid line. Visually it now reads as a real newspaper crossword instead of separated tiles.
- **No more "Game Over"** — the old `finale` step (Play Again button, big "WINNER!" with confetti) was split into:
  - `winner` — the editorial result card with a "Continue →" button.
  - `segue` — the clean "Coming Up Next" bumper that fades in over ~1 s as the winner card fades out.
  Step count went from 23 to **25**.
- **No default HTML buttons anywhere** — every interactive element (start, point, back, continue, next, restart) is custom-styled with editorial type, color blocks, and soft hover lifts.

---

## Customizing for your class

Everything you'd want to swap is clearly marked with `PLACEHOLDER` comments.

### Player names (top of `<body>`)
Inside the `<div class="scoreboard">` block, replace the `<span>Player 1</span>` etc. lines with your students' names.

### Team names
Same scoreboard — change `<div class="team-label">Team A</div>` / `Team B` and the `team-tag` lines if you want different team identifiers.

### Hints, video URLs, guest names (top of `<script>`)
The `WORDS` array. Each entry has:
```js
{ id: 1, word: "SUPERMERCADO", direction: "horizontal", startRow: 3, startCol: 1,
  hint:    { kind: "text", value: "..." },                  // or kind: "emojis" / "none"
  feature: { kind: "video", url: "https://www.youtube.com/embed/..." } // or kind: "guest"
},
```
- `hint.kind`: `"text"` (string), `"emojis"` (array of 3), or `"none"` (presenter cue).
- `feature.kind`: `"video"` (with `url`, use the `/embed/` form) or `"guest"` (with `name` and `initial`).

### Audio (theme, logo flash, reveal)
Three `<audio>` tags inside the stage. Drop your files next to `index.html` and update the three `REPLACE_WITH_*.mp3` `src` values.

### "Coming Up Next" activity title
At the bottom of `<body>`, inside the segue overlay:
```html
<div class="segue-up-next-title" id="up-next-title">Conversation Practice</div>
```
Change the text to whatever class activity is coming up next (or remove it entirely).

### Want different words?
The grid coordinates are hardcoded for these specific four words and the four shared-letter intersections. Swapping in different words means re-deriving `direction`, `startRow`, `startCol` so they cleanly intersect and the shared cells agree on letters. The full ASCII layout diagram is in the comment above the `WORDS` array — start there.

---

## Host controls

| Action | How |
|---|---|
| Advance to next step | **Spacebar** or click the floating **Next ▸** in the bottom-right |
| Award a point | Click **+1 Team A** or **+1 Team B** on the reveal screen (also advances) |
| Return to board after a feature | Click **↩ Back to the Board** or press Spacebar |
| End the segment gracefully | Click **Continue →** on the winner screen — fades into "Coming Up Next" |
| Run the show again | Tiny **↺ Restart Show** button in the bottom-right of the segue |

The reveal step blocks Spacebar / Next on purpose — you must click one of the Award-Point buttons to award and advance. Auto-advance steps (countdown, intro logo, logo flash) ignore Spacebar.

---

## Tech details

- **Single file**, no build step, no JS dependencies. Google Fonts (`Playfair Display`, `Inter`) is `@import`-ed; if offline, fonts fall back to Georgia / system sans.
- **16:9 fixed stage** — `1920×1080` rendered internally, scaled with `transform: scale(min(w/1920, h/1080))` on each window resize. Layout is pixel-correct on any monitor or projector.
- **State machine** — flat 25-step `STEPS` array with `state.step` index. `applyStep()` is the single source of truth for what the screen looks like.
- **Crossword** — 13×12 CSS-grid with `gap: 0`. Cells with no word use `background: transparent`; cells with letters have `outline: 1.5px solid var(--ink)` so adjacent cells share the same line. Intersections render as one DOM cell.
- **Cross-fade** between winner and segue — winner gets a `.fading` class that animates `opacity → 0` over 0.9 s; segue's `.active` runs a `segueFadeIn` keyframe (~1.2 s).

---

## License
Use it however you want for your class. No warranty.
