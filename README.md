# EL GRAN SHOW — 2026 Edition

A single-file, **broadcast-grade** TV game show for a Spanish class. The stage is built like a 2026 evening-broadcast set: a deep navy void washed by cyan + magenta aurora gradients, drifting purple orbs, a faint 2-pixel grid, animated film-grain, glass overlays with neon trim, and giant Space Grotesk display type. Two student teams compete to guess **four physically-intersecting Spanish words** on a real crossword grid (`SUPERMERCADO`, `JOYERÍA`, `PROFESORA`, `INGENIERO`).

The host advances through a 29-step broadcast with the floating **Next ▸** pill or the **Spacebar**, and can step backward with the **◂ Back** pill or **Left-arrow / Backspace**. **All sound effects are synthesized in the browser via the Web Audio API** — no audio files needed; everything works the moment you open `index.html`.

---

## What's on screen

- **2026 dark broadcast palette** — `#070b1a` base, cyan `#00e5ff`, magenta `#ff3d83`, violet `#8b5cf6`, ivory `#f5f7fc`, soft amber + lime for special accents. Team A is cyan, Team B is magenta. No light/dark mode split — one cohesive set palette.
- **Modern type** — `Space Grotesk` for everything display (titles, scores, headlines, banners) and `Inter` for body / small UI. Both loaded from Google Fonts with safe fallbacks.
- **Animated stage backdrop** — a multi-axis aurora made of three blurred radial gradients drifting slowly (`auroraDrift`), a soft 64-px grid masked to a centre vignette, animated grain, and **five blurred drifting orbs** (cyan / magenta / violet) that float around the entire show as ambient atmosphere.
- **Continuous neon dust motes** — small glowing specks in cyan / magenta / violet / white drift upward across the stage at all times.
- **Glass overlays** — every overlay (hint, reveal, celebration, feature, winner, segue) is a translucent dark card with `backdrop-filter: blur()` and a neon border, with a thin animated cyan→magenta hairline along the top.
- **Big chunky pill controls** — pill buttons for Begin Broadcast, +1 Team A / B, Continue, Next, Back, Play; outline pills for Stop and Restart. Hover sheen + lift on every one.
- **Big crossword cells** (64 px each) on a dark glass board, with a slow neon light-sweep (`cardSweep`) passing diagonally across the card every 7 s.
- **Team-coloured highlighting** — when a word is the focus, its cells stagger-pop in, glow cyan, and the **head cell's number badge zooms 2.6× with a glow halo**. A floating "№ N · Word · Horizontal · 12 Letras" banner pops in below the crossword.

---

## The 29-step flow

1. **Pre-show** — animated cyan-to-magenta gradient title, ON AIR · 2026 pill, Begin Broadcast button (gradient pill with rotating sheen).
2. **Countdown** — 3 / 2 / 1 in giant Space Grotesk inside three concentric counter-spinning neon rings; each digit fires a beep, "1" rolls into a sweep + fanfare.
3. **Intro logo** — animated logo card with cyan→magenta gradient text, music-box fanfare. Auto-advances.
4. **Per word, 6 sub-steps × 4 words = 24 steps:**
   - **Highlight** — staggered cell pop on the word, head cell's number badge zooms up, floating word banner ("№ 1 · Palabra · Horizontal · 12 Letras") slides in below the board.
   - **Hint** — glass card slides in:
     - W1 (`SUPERMERCADO`): text hint (Spanish).
     - W2 (`JOYERÍA`): three large emojis (💍 💎 ⌚) with rotation pop-in.
     - W3 / W4 (`PROFESORA`, `INGENIERO`): no overlay; a cyan presenter cue near the bottom signals the host to introduce the word live.
   - **Reveal** — full-screen gradient answer word with halo, sparkles, confetti, **streaks** (radial speed lines), 4-note triumphant chord, and the two custom +1 buttons.
   - **Celebration** *(new)* — clicking +1 advances here automatically. A full-screen panel **eases in** showing the scoring team's gradient name, a giant `+1` sting, the team's new score, and the player names. Particle burst (100 confetti + 10 streaks in the team colour) + a celebratory sting. After 2.4 s, the panel **eases out** and the scoreboard underneath shows a 2-second glow on the team's score. Then Next becomes available.
   - **Intro replay** *(new)* — pressing Next plays the show intro card again with theme music. Auto-advances after 2.2 s into the feature.
   - **Feature** —
     - W1 / W2 (video): a **host-controlled URL input** appears, pre-filled with the suggested YouTube URL. The host can paste any URL (`youtu.be/...`, `youtube.com/watch?v=...`, or already-`/embed/`) and click ▶ Play. The iframe loads with autoplay. A small Stop · Edit URL pill returns to the input.
     - W3 / W4 (guest): a glass guest card with a circular cyan-ringed photo placeholder showing the guest's initial, their name in 5-rem display type (**Sarra Gharbi** for Profesora, **Majd Lahbib** for Ingeniero), profession label, and the word.
     - Both have a "↩ Back to the Board" pill to advance.
5. **Winner** — gradient team name (cyan ramp for A, magenta ramp for B, violet ramp for tie), animated rule, glass final-scores card, gradient Continue button. Confetti + streaks + 7-note victory fanfare with applause noise.
6. **Segue** — graceful 1.2 s fade-in of a clean station bumper: gold-style mark, gradient "EL GRAN SHOW" centred, "Más a continuación · Up Next: <activity>", thanks line, and a small **↺ Restart Show** pill in the bottom-right.

The video and guest words are intentionally split — videos for the first two words (so you have a media moment), then **guests live in the studio for the last two words** so the segment closes with people on camera.

---

## Sound design (synthesized)

Generated live by the Web Audio API; no audio files required.

| Cue | Trigger |
|---|---|
| Click | Spacebar / Next / Begin Broadcast / Stop |
| Back | Back button / Left-arrow / Backspace |
| Beep | Each countdown digit (3, 2, 1, with rising pitch) |
| Go | After "1" — sawtooth sweep + noise burst into intro |
| Fanfare | Intro logo + intro replay steps |
| Shimmer | Word highlight + scoreboard score-glow + segue fade-in |
| Swoosh | Hint / feature card slides in + ▶ Play video |
| Tick | Each cell pop on highlight + each letter fill on reveal |
| Reveal | 4-note major chord + bell tail + cymbal-ish noise |
| Point | Cheerful chime on +1 (different pitch per team) |
| Celebrate | Team-coloured 4-note arpeggio + sub bass + noise tail |
| Whoosh | Reserved for short transitions |
| Victory | 7-note grand fanfare + 3 layered applause noise bursts |

Mute everything with the round **♪ / ✕** pill (top-right) or **M**.

---

## Host controls

| Action | How |
|---|---|
| Advance | **Spacebar** or click the floating **Next ▸** pill (gradient cyan) |
| Step back | Click **◂ Back** pill, or press **Left-arrow** / **Backspace**. Stepping back across a reveal undoes the awarded point. |
| Award a point | Click **+1 Team A** (cyan) or **+1 Team B** (magenta) on the reveal screen |
| Play the video | Type or paste a URL in the input, click **▶ Play** |
| Stop the video | **■ Stop · Edit URL** pill — returns to the URL input |
| Return to board | **↩ Back to the Board** or Spacebar |
| Continue from winner | Click **Continue →** — fades into "Coming Up Next" |
| Restart show | Tiny **↺ Restart Show** button on the segue |
| Mute / unmute | Round **♪ / ✕** pill (top-right) or **M** |

The reveal step blocks Spacebar / Next on purpose — you must click +1 to award and advance. Auto-advance steps (countdown, intro logo, intro replay) ignore Spacebar. The Spacebar / Backspace / M shortcuts also yield to text inputs (so you can type a URL without it being captured as a control).

---

## Customizing for your class

### Player names (top of `<body>`)
Inside `<div class="scoreboard">`, replace the `<span>Player 1</span>` etc. lines with your students' names. The celebration overlay reads from `getPlayerNames("a"|"b")` in the JS — keep that in sync if you want the celebration card to display real names.

### Team labels
Same scoreboard — change `Team A` / `Team B` and the matching `team-tag` lines.

### Hints, video URLs, guests (top of `<script>`)
The `WORDS` array. Each entry:
```js
{
  id: 1,
  word: "SUPERMERCADO",
  direction: "horizontal", startRow: 3, startCol: 1,
  hint:    { kind: "text", value: "..." },                        // or kind: "emojis" / "none"
  feature: { kind: "video", url: "https://www.youtube.com/embed/YiqBolcimm0" }
                                                                  // or kind: "guest", name: "...", initial: "..."
}
```
- `hint.kind` — `"text"`, `"emojis"` (array of 3), or `"none"` (presenter cue under the board).
- `feature.kind` — `"video"` (with `url`; the input accepts `youtu.be/...`, `youtube.com/watch?v=...`, or `/embed/...`) or `"guest"` (with `name` and `initial`).

Current guests:
- **Profesora** — Sarra Gharbi (live in studio).
- **Ingeniero** — Majd Lahbib (live in studio, ingeniero).

### Optional theme song
The single `<audio id="theme-song">` slot is retained for an optional looping theme. Drop your file next to `index.html` and replace `REPLACE_WITH_THEME_SONG.mp3`. All other sound is synthesized.

### "Coming Up Next" activity
Edit `#up-next-title` near the bottom of `<body>` to whatever class activity follows.

### Want different words?
The grid coordinates are hardcoded for these specific four intersecting words. Swapping them means re-deriving `direction`, `startRow`, `startCol` so they cleanly intersect and shared cells agree on letters. Auto-layout is intentionally not done here.

---

## Tech details

- **Single file**, no build step, no JS dependencies. Google Fonts (`Space Grotesk`, `Inter`) `@import`-ed; offline fallback to system sans-serif.
- **16:9 fixed stage** — `1920×1080` rendered internally, scaled with `transform: scale(min(w/1920, h/1080))` on every window resize. Letterbox cleanly fills with the dark surround on any monitor (laptop, classroom, 4K projector).
- **State machine** — flat 29-step `STEPS` array. `applyStep()` is the single source of truth. Transition timers are tracked in `state.pendingTimeout` / `state.pendingTimeout2` and cleared on every step change.
- **Score history stack** — every awarded point is pushed onto `state.scoreHistory`. Stepping back across a reveal pops it and decrements the team that scored.
- **Crossword** — 13×12 CSS grid, `gap: 0`, 64 px cells. Cells with no word are transparent; cells with letters are dark glass with a 1.5 px white outline; intersections render as one DOM cell.
- **Celebration step** — a single step with two CSS transition phases: `.active` → 2.4 s → `.shrinking` → 1.2 s → cleanup. The scoreboard underneath gets a `scoreGlow` keyframe at the same moment the panel begins to ease out.
- **Video URL input** — `toEmbedUrl()` extracts the YouTube ID from `youtu.be/<id>` or `?v=<id>` and rebuilds an `/embed/<id>?autoplay=1&rel=0` URL.
- **Web Audio SFX** — `osc()` + `noise()` helpers build oscillator and filtered-noise graphs on demand. Audio context unlocks on the first user gesture (Begin Broadcast). The `sfx` object exposes `click`, `back`, `tick`, `beep`, `go`, `fanfare`, `shimmer`, `swoosh`, `whoosh`, `reveal`, `point(team)`, `celebrate(team)`, `victory`.
- **Particles** — `spawnConfetti`, `spawnSparkles`, `spawnStreaks` (radial speed lines), continuous ambient `dust`, and 5 large blurred floating `orb` divs.
- **Keyboard shortcuts** — Space (advance), Left-arrow / Backspace (back), M (mute toggle). All yield when focus is in an `<input>` or `<textarea>` so the URL field works normally.

---

## License
Use it however you want for your class. No warranty.
