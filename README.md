# EL GRAN SHOW — Studio Edition

A single-file, **broadcast-grade** TV game show for a Spanish class. The stage now looks like a real evening-game-show set: a deep teal-navy backdrop bathed in spotlights, gold marquee bulbs blinking around the frame, drifting dust motes in the warm light, and ivory crossword cards with gold trim that catch a slow light-sweep. Two student teams compete to guess **four physically-intersecting Spanish words** on a real crossword grid (`SUPERMERCADO`, `JOYERÍA`, `PROFESORA`, `INGENIERO`). The presenter advances the broadcast one beat at a time via a small bottom-right control or the spacebar, and can step **backward** with a Back button or the left-arrow / Backspace key.

The whole show is one self-contained `index.html`. No build step, no server, no dependencies. **All sound effects are synthesized with the Web Audio API** — no external mp3 files needed; everything works the moment you open the file. (Google Fonts is loaded by `@import` for the premium typography; falls back gracefully when offline.)

---

## The look

- **Theatrical jewel-tone palette** — deep teal-navy stage `#07182b → #0e2a47 → #14406b`, rich purple wash, warm gold bulbs `#e6b34a / #f5d77f`, coral `#ee5b3c`, ruby `#a02e3a`, mint `#3ab59f`, ivory paper cards `#fdf7e6`. Not "light vs dark mode" — one cohesive studio-set palette.
- **Gold marquee bulbs** blinking around all four edges of the stage (~100 bulbs, randomised blink offsets) — the classic game-show frame.
- **Sweeping spotlight beams** projected from the top of the stage; they slowly drift left-right via a `beamSweep` keyframe animation, mixed in screen-blend so they tint the backdrop without washing it out.
- **Drifting dust motes** — small glowing specks in gold / ivory / mint / coral float upward across the stage continuously, like the haze of a TV-studio set under hot lights.
- **Animated gradient text** — every headline (show title, intro logo, reveal word, winner team, segue logo) uses a gold-to-amber gradient with a drop-shadow glow that pulses or shimmers.
- **Light sweep across the crossword card** — a slow diagonal highlight passes across the ivory crossword card every few seconds (`cardSweep` animation).
- **Newspaper-style crossword** — `gap: 0` so cells physically touch and share their outlines at the four intersections. Filled cells turn into an animated gold gradient on flip-in.
- **Custom buttons everywhere** — gold pill buttons with rotating sheen highlight, deep-blue ctrl pills, no default browser styling.
- **Animated chyron headers** — the scoreboard slides in from above, hint / guest cards have animated header bars with light tails, and the score number performs a wobble + colour pop on bump.
- **Paper confetti** in editorial colours rains during reveal and winner moments.

---

## Sound effects (synthesized)

Every cue is generated live by the browser's Web Audio API — there are no audio files to drop in for the SFX (the optional theme-song slot remains).

| Cue | Trigger |
|---|---|
| Click | Spacebar / Next / Begin Broadcast |
| Back | Back button / left-arrow / Backspace |
| Beep | Each countdown digit (3, 2, 1) |
| Go | After "1" — rising sweep + fanfare into intro |
| Fanfare | Intro logo step |
| Shimmer | Word highlight + segue fade-in |
| Swoosh | Hint / feature card slides in |
| Tick | Each crossword cell flips during reveal |
| Reveal | Triumphant 4-note chord + bell tail on answer reveal |
| Whoosh | Logo flash transition |
| Point | Cheerful chime when +1 is awarded (different pitch per team) |
| Victory | 6-note grand fanfare + applause-like noise on the winner card |

Sound can be **muted** with the round ♪ pill in the top-right corner or the **M key**.

---

## The flow (25 steps, presenter-paced)

1. **Pre-show** — backdrop with spotlight pool, "ON AIR" pulsing pill, gold rules, big gold-gradient "EL GRAN SHOW" headline, pulsing "Begin Broadcast" gold button.
2. **Countdown** — 3 / 2 / 1 in serif inside a spinning gold ring (one ring rotates clockwise, an inner dashed ring counter-clockwise); each digit fires a beep, "1" plays a fanfare.
3. **Intro logo** — animated full-stage logo card with thin gold rules above and below; rising fanfare.
4. **Per word, 5 sub-steps × 4 words = 20 steps:**
   - **Highlight** — current word's tiles tint to gold and pulse, with a shimmer chime.
   - **Hint** — chyron header slides in, then:
     - W1 (`SUPERMERCADO`): text hint.
     - W2 (`JOYERÍA`): three large emojis (💍 💎 ⌚) that pop in with a rotation.
     - W3 / W4 (`PROFESORA`, `INGENIERO`): no overlay; a small "El presentador introduce la palabra" cue under the board signals the host to introduce the word live.
   - **Reveal** — gold-gradient answer card with shimmer halo, sparkle particles, paper confetti, triumphant chord; two custom Award-Point buttons (one navy for Team A, one coral for Team B). Letters fill into the crossword with a card-flip animation in the background, each letter ticking as it lands.
   - **Transition** — short logo flash overlay (1.5 s + whoosh sound), auto-advances.
   - **Feature** —
     - W1 / W2: "Broadcast · Video" — gold-bordered ivory card around a YouTube iframe with a glowing red "EN VIVO" pill above.
     - W3 / W4: "Invitado en Vivo · Live Studio Guest" — chyron card with a circular gold-ringed photo placeholder, the guest's name, profession label, and the word.
     - Both have a "↩ Back to the Board" button.
5. **Winner** — gold-gradient team name, paper-confetti rain, 6-note victory fanfare with applause, final-scores card, and a "Continue →" button.
6. **Segue ("Coming Up Next")** — graceful 1.2 s fade-in of a clean station bumper: gold rules, "EL GRAN SHOW" centred, "Más a continuación · Up Next: <activity>", "Gracias por jugar · Thank you for playing", and a small "↺ Restart Show" pill in the bottom-right.

---

## Host controls

| Action | How |
|---|---|
| Advance to next step | **Spacebar** or click the floating gold **Next ▸** pill in the bottom-right |
| **Step backward** | Click the **◂ Back** pill, or press **Left-arrow** / **Backspace**. Going back across a reveal that awarded a point will undo that point. |
| Award a point | Click **+1 Team A** or **+1 Team B** on the reveal screen (also advances) |
| Return to board after a feature | Click **↩ Back to the Board** or press Spacebar |
| End the segment gracefully | Click **Continue →** on the winner screen — fades into "Coming Up Next" |
| Run the show again | Tiny **↺ Restart Show** button in the bottom-right of the segue |
| Mute / unmute | Round **♪ / ✕** pill (top-right) or press **M** |

The reveal step blocks Spacebar / Next on purpose — you must click one of the Award-Point buttons to award and advance. Auto-advance steps (countdown, intro logo, logo flash) ignore Spacebar.

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

### Optional theme song
A single `<audio id="theme-song">` tag is retained for an optional looping theme. Drop your file next to `index.html` and replace `REPLACE_WITH_THEME_SONG.mp3`. All other sound effects are synthesized — no other audio files needed.

### "Coming Up Next" activity title
At the bottom of `<body>`, inside the segue overlay:
```html
<div class="segue-up-next-title" id="up-next-title">Conversation Practice</div>
```

### Want different words?
The grid coordinates are hardcoded for these specific four words and the four shared-letter intersections. Swapping in different words means re-deriving `direction`, `startRow`, `startCol` so they cleanly intersect and the shared cells agree on letters. The full ASCII layout diagram is in the comment above the `WORDS` array — start there.

---

## Tech details

- **Single file**, no build step, no JS dependencies. Google Fonts (`Playfair Display`, `Inter`) is `@import`-ed; if offline, fonts fall back to Georgia / system sans.
- **16:9 fixed stage** — `1920×1080` rendered internally, scaled with `transform: scale(min(w/1920, h/1080))` on each window resize. Layout is pixel-correct on any monitor or projector — laptops, classroom displays, and 4K screens all letterbox cleanly with the dark theatrical surround filling the gutters.
- **State machine** — flat 25-step `STEPS` array with `state.step` index. `applyStep()` is the single source of truth for what the screen looks like.
- **Score history stack** — every awarded point is pushed onto `state.scoreHistory`. Stepping back across a reveal pops the stack and decrements the team that scored, so accidental presses are recoverable.
- **Crossword** — 13×12 CSS-grid with `gap: 0`. Cells with no word are transparent; cells with letters have `outline: 1.5px solid` so adjacent cells share gridlines. Intersections render as one DOM cell.
- **Web Audio API SFX** — `osc()` and `noise()` helpers build oscillator-and-filter graphs on demand; the audio context is unlocked on the first user gesture (the Begin Broadcast button) per browser policy.
- **Marquee bulbs** are 100 individual DOM elements with random animation-delays so they blink out-of-phase. Cheap to render and pure CSS animations.
- **Ambient dust motes** spawn every 280 ms, drift upward over 10–22 s, then self-remove.
- **Cross-fade** between winner and segue — winner gets `.fading` (opacity → 0 over 0.9 s); segue runs a `segueFadeIn` keyframe (~1.2 s).

---

## License
Use it however you want for your class. No warranty.
