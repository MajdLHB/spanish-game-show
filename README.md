# RINCONES DEL MUNDO — 2026

A single-file, **broadcast-grade** TV game show for a Spanish A1 / A2 class. The stage is built like a 2026 evening-broadcast set: deep navy void washed by cyan + magenta aurora gradients, drifting purple orbs, faint 64-px grid, animated film-grain, glass overlays with neon trim, giant Space Grotesk display type, and an **animated "corners-of-the-world" logo** — four pulsing L-shaped corner brackets framing a rotating globe (cyan / violet / magenta gradient sphere with equator and meridian rings).

Two student teams compete to guess **four physically-intersecting Spanish words** on a real crossword grid (`SUPERMERCADO`, `JOYERÍA`, `PROFESORA`, `INGENIERO`):

- **Equipo A — Aouadi** (1 player)
- **Equipo B — Siwar y Ahlem** (2 players)

The host advances through a **37-step broadcast** with the floating **Siguiente ▸** pill or the **Espacio** key, and can step backward with the **◂ Atrás** pill or **Left-arrow / Backspace**. **All sound effects are synthesized in the browser via the Web Audio API** — open `index.html` and the show works offline. Every visible word is in Spanish (A1 / A2 friendly); there are no English subtitles in the on-air UI.

---

## What's on screen

- **Animated "Rincones del Mundo" logo** — four L-shaped corner brackets (alternating cyan / magenta) pulsing outward on a 2.6 s cycle around a rotating globe with conic-gradient sphere, equator ellipse, and meridian ellipse. Used in pre-show, the opening logo splash, intro replay, the closing trip bumper, and the final logo loop. Three sizes (`.world-logo` / `.md` / `.sm`) via a single `--size` custom property.
- **Two floating glass team widgets** in the top corners — Aouadi (cyan) anchored top-left, Siwar y Ahlem (magenta) anchored top-right. Each ~420 × 130, with a colour stripe accent, eyebrow tag (`EQUIPO A` / `EQUIPO B`), team name in display type, player names, and a giant tabular score number. Replaces the old top chyron header bar so the crossword has more breathing room.
- **2026 dark broadcast palette** — `#070b1a` base, cyan `#00e5ff`, magenta `#ff3d83`, violet `#8b5cf6`, ivory `#f5f7fc`. Equipo A is cyan, Equipo B is magenta.
- **Modern type** — `Space Grotesk` for everything display (titles, scores, headlines, banners, badges) and `Inter` for body / small UI.
- **Animated stage backdrop** — multi-axis aurora made of three blurred radial gradients drifting slowly, soft 64-px grid masked to a centre vignette, animated grain, and **five blurred drifting orbs** as ambient atmosphere.
- **Continuous neon dust motes** — small glowing specks in cyan / magenta / violet / white drift upward across the stage at all times.
- **Glass overlays** — every overlay (hint, reveal, celebration, scoreboard-full, feature, winner, trip bumper, end loop) is a translucent dark card with `backdrop-filter: blur()` and a neon border + thin animated cyan→magenta hairline along the top.
- **Big chunky pill controls** — `Empezar`, `+1 Aouadi`, `+1 Siwar y Ahlem`, `Continuar`, `Siguiente`, `Atrás`. Hover sheen + lift on every one.
- **Big crossword cells** (64 px each) on a dark glass board, with a slow neon light-sweep passing diagonally every 7 s.
- **Team-coloured highlighting** — when a word is the focus, its cells stagger-pop in, glow cyan, and the head cell's number badge zooms 2.6× with a glow halo. A floating "№ N · Palabra · Horizontal · 12 Letras" banner pops in below the crossword.

---

## The 37-step flow

Opening sequence:
1. **Pre-show** — animated logo + cyan-to-magenta gradient title (`RINCONES / DEL MUNDO`), `EN VIVO · 2026` pill, `Empezar` button (gradient pill with rotating sheen). The first user click here also unlocks the Web Audio context so all subsequent SFX play.
2. **Cuenta atrás** — 3 / 2 / 1 in giant Space Grotesk inside three concentric counter-spinning neon rings; each digit fires a beep, "1" rolls into a sweep + fanfare.
3. **Logo splash** *(NEW)* — animated logo + gradient title, music-box fanfare. Auto-advances after ~2.5 s.
4. **Equipo A intro** *(NEW)* — full-stage cyan reveal: `★ EQUIPO A ★` mark, big circular `A` badge with shine + bob, gradient `AOUADI` name, single-player roster, 80 cyan confetti + 10 streaks + celebrate sting. Auto-advances after ~3.5 s.
5. **Equipo B intro** *(NEW)* — same but magenta, `B`, `SIWAR Y AHLEM`, two-player roster.
6. **Logo splash again** *(NEW)* — short ~2 s reprise before the words begin.

Per word, 7 sub-steps × 4 words = **28 steps**:
- **Highlight** — staggered cell pop on the word, head cell's number badge zooms up, floating word banner ("№ 1 · Palabra · Horizontal · 12 Letras") slides in below the board.
- **Pista** — glass card slides in with the hint:
  - W1 SUPERMERCADO: emojis 🛒 🥬 💰
  - W2 JOYERÍA: emojis 💍 💎 ⌚
  - W3 PROFESORA: text "Trabajo en una escuela. Enseño a los estudiantes."
  - W4 INGENIERO: text "Uso las matemáticas para construir edificios y puentes."
  Every word now has a visual hint — there is no "presenter cue" mode anymore.
- **Respuesta** — full-screen gradient answer word with halo, sparkles, confetti, streaks, 4-note triumphant chord, and the two custom +1 buttons (`+1 Aouadi`, `+1 Siwar y Ahlem`).
- **Celebración** — clicking +1 advances here automatically. Full-screen panel eases in with the scoring team's gradient name, a giant `+1 Punto` sting, the team's new score, and the player names. Particle burst (100 confetti + 10 streaks in the team colour) + a celebratory sting. After 2.4 s the panel eases out and the floating team widget gets a 2-second score-glow. After another 1.2 s the show **auto-advances** into the fullscreen scoreboard.
- **Marcador (scoreboard full-screen)** *(NEW)* — full-stage panel with two side-by-side glass cards: `Equipo A · Aouadi · score`, divider, `Equipo B · Siwar y Ahlem · score`. The 12 rem score numbers are gradient-clipped in each team's colour. Waits for `Siguiente`.
- **Intro replay** — short logo reprise with theme music. Auto-advances after 2.2 s into the feature.
- **Feature** —
  - W1 / W2 (video): the iframe loads with `?autoplay=1&rel=0` the moment the step opens — no URL input, no Play / Stop. Audio is hard-stopped (`iframe.src = ""`) on every step transition (forward, back, or auto), so video sound never bleeds into the next step.
  - W3 / W4 (guest): a real lower-third TV chyron — top strip with a pulsing red `EN VIVO` chip + 5-bar animated equalizer + 🎙 + show name; centre body with cyan/magenta colour stripes flanking the guest name (5.5 rem) and a pill-style title (`PROFESORA` / `INGENIERO`); bottom strip with `★ Rincones del Mundo · En Vivo ★` + `Palabra · <WORD>`. No profile photo — it reads like a real broadcast lower third.
  - Both have an `↩ Al Tablero` pill that advances.

Closing sequence:
- **Ganador** *(UPGRADED)* — 🏆 trophy with zoom + bob, gradient team name (12 rem, cyan ramp for Aouadi, magenta for Siwar y Ahlem, violet for `Empate`), animated rule, glass final-scores card with 5.5 rem totals, gradient `Continuar →` button. 220 confetti + 26 streaks + 80 sparkles. Longer 11-note victory motif with sub-bass and a sustained C-major chord finale plus four layered applause noise bursts.
- **Viaje a Turquía** *(NEW)* — looping broadcast bumper. Sky gradient (deep blue → cyan → ochre → orange), 4 drifting clouds, two ✈️ airplanes flying L→R on offset delays with a contrail, big gradient `VIAJE A TURQUÍA` headline pulse, and a `Rincones Travel · Agencia de Viajes · Estambul · Capadocia` agency banner. A synthesised airplane-engine pad (sawtooth + sine + lowpassed noise + 6 Hz tremolo) loops while the step is active. Stays animating until `Siguiente`.
- **End loop** *(NEW)* — controls hide; the screen settles into a gentle infinite loop of the world-logo (bobbing) with the `Rincones del Mundo` tagline below. A calm low-volume sine pad plays a slow `C → Am → F → G` chord progression on a 6.5 s loop with smooth frequency ramps. The only way out is the small `↺ Reiniciar` pill in the bottom-right (page reload).

---

## Sound design (synthesized)

Generated live by the Web Audio API; no audio files required. Audio context unlocks on the first user gesture (`Empezar` click).

| Cue | Trigger |
|---|---|
| Click | Espacio / Siguiente / Empezar |
| Back | Atrás / Left-arrow / Backspace |
| Beep | Each cuenta-atrás digit (3, 2, 1, rising pitch) |
| Go | After "1" — sawtooth sweep + noise burst into the splash |
| Fanfare | Logo splash + intro replay |
| Shimmer | Word highlight + scoreboard score-glow + scoreboard fullscreen |
| Swoosh | Pista / feature card slides in |
| Tick | Each cell pop on highlight + each letter fill on respuesta |
| Reveal | 4-note major chord + bell tail + cymbal-ish noise |
| Point | Cheerful chime on +1 (different pitch per team) |
| Celebrate | Team-coloured 4-note arpeggio + sub-bass + noise tail (also fires on team intros) |
| Victory | 11-note rising fanfare + sustained C-major chord finale + sub-bass + four applause bursts |
| Engine pad | Looped airplane rumble during the trip bumper (sawtooth + sine + lowpassed noise + 6 Hz tremolo) |
| End-loop pad | Calm C / Am / F / G sine progression during the final loop |

Mute everything with the round **♪ / ✕** pill (bottom-left of the stage) or **M**. Both looped pads stop on mute and re-fire on un-mute only if the current step still calls for them.

The optional `<audio id="theme-song">` slot is retained — drop a file next to `index.html` and replace `REPLACE_WITH_THEME_SONG.mp3`. Its volume is **capped at 0.4** in code so it never drowns out the synthesized SFX.

---

## Host controls

| Action | How |
|---|---|
| Advance | **Espacio** or click the floating **Siguiente ▸** pill (gradient cyan) |
| Step back | Click **◂ Atrás** pill, or press **Left-arrow** / **Backspace**. Stepping back across a respuesta undoes the awarded point. |
| Award a point | Click **+1 Aouadi** (cyan) or **+1 Siwar y Ahlem** (magenta) on the respuesta screen |
| Return to board | **↩ Al Tablero** or Espacio |
| Continue from ganador | **Continuar →** — fades into the trip bumper |
| Restart show | Tiny **↺ Reiniciar** pill on the end-loop |
| Mute / unmute | Round **♪ / ✕** pill (bottom-left) or **M** |

The respuesta step blocks Espacio / Siguiente on purpose — you must click +1 to award and advance. The `end_loop` step is also non-advanceable; only the `↺ Reiniciar` pill exits. Auto-advance steps (cuenta atrás, logo splash, team intros, intro replay, celebración → marcador) ignore Espacio.

---

## Stage scaling

The stage renders internally at a fixed `1920 × 1080` and is scaled to fit the viewport with `transform: scale(min(w/1920, h/1080) * 0.97)`. The `0.97` safety multiplier keeps a small breathing margin around the edges so the floating bottom-right controls and bottom-left mute pill never reach the viewport boundary on tight or letterboxed displays. `scaleStage()` re-fires on `resize`, `orientationchange`, and once on `load`.

---

## Customizing for your class

### Team identity (top of `<body>`)
Inside `<div class="scoreboard">`, the two `.team-card` blocks hold the team eyebrow, name, and player list. Swap `Aouadi` and `Siwar y Ahlem` for your own team names. The celebration / team-intro / scoreboard-full / winner overlays read from `getPlayerNames("a"|"b")` and `getTeamName("a"|"b")` in the JS — keep those in sync if you want the on-air panels to display real names.

### Hints, video URLs, guests (top of `<script>`)
The `WORDS` array. Each entry:
```js
{
  id: 1,
  word: "SUPERMERCADO",
  direction: "horizontal", startRow: 3, startCol: 1,
  hint:    { kind: "emojis", value: ["🛒", "🥬", "💰"] },           // or kind: "text", value: "..."
  feature: { kind: "video",  url: "https://www.youtube.com/embed/YiqBolcimm0" }
                                                                   // or kind: "guest", name: "...", title: "PROFESORA"
}
```
- `hint.kind` — `"emojis"` (array of 3) or `"text"`. The old `"none"` mode has been removed; every word must have a visible hint.
- `feature.kind` — `"video"` (with `url`; `toEmbedUrl()` accepts `youtu.be/<id>`, `youtube.com/watch?v=<id>`, or already-`/embed/<id>`) or `"guest"` (with `name` and `title`).

Current guests:
- **Profesora** — Sarra Gharbi (live in studio).
- **Ingeniero** — Majd Lahbib (live in studio).

### Optional theme song
The `<audio id="theme-song">` slot still exists for an optional looping theme. Drop a file next to `index.html` and replace `REPLACE_WITH_THEME_SONG.mp3`. Its playback volume is capped at 0.4 in `tryPlay()` so the synthesized SFX always sit on top.

### Want different words?
The grid coordinates are hardcoded for these specific four intersecting words. Swapping them means re-deriving `direction`, `startRow`, `startCol` so they cleanly intersect and shared cells agree on letters. Auto-layout is intentionally not done here.

### Want a different logo?
The logo is built entirely in CSS (`.world-logo`, `.wl-bracket.*`, `.wl-globe`, `.wl-globe-sphere`). The bracket size, globe size, and overall footprint scale from a single `--size` custom property (also exposed via the `.md` and `.sm` modifier classes). Swap colours via the `--cyan`, `--magenta`, `--violet` design tokens at the top of the CSS.

---

## Tech details

- **Single file**, no build step, no JS dependencies. Google Fonts (`Space Grotesk`, `Inter`) `@import`-ed; offline fallback to system sans-serif.
- **16:9 fixed stage** — `1920 × 1080` rendered internally, scaled with `transform: scale(min(w/1920, h/1080) * 0.97)` on every window resize / orientationchange / load. The `0.97` safety multiplier guarantees the floating controls and sound toggle never get clipped on edge cases.
- **State machine** — flat 37-step `STEPS` array. `applyStep()` is the single source of truth. Transition timers are tracked in `state.pendingTimeout` / `state.pendingTimeout2` and cleared on every step change.
- **Score history stack** — every awarded point is pushed onto `state.scoreHistory`. Stepping back across a respuesta pops it and decrements the team that scored.
- **Crossword** — 13 × 12 CSS grid, `gap: 0`, **64 px** cells. Cells with no word are transparent; cells with letters are dark glass with a 1.5 px white outline; intersections render as one DOM cell.
- **Floating team widgets** — two `position: absolute` glass cards anchored to the stage's top-left and top-right at 24 px inset. The wrapping `.scoreboard` element is `position: absolute; inset: 0; pointer-events: none` so the widgets sit on top of overlays without blocking clicks; the cards themselves restore `pointer-events: auto`. The score elements keep their original `id="score-a"` / `id="score-b"` so the existing `bump` / `glow` animations fire unchanged.
- **World logo** — `.world-logo` is a CSS-only assembly: four `.wl-bracket` corner pieces using two pseudo-elements each to draw an L, four diagonals offset on a `wlBracket` keyframe, and a `.wl-globe` clipping mask containing a spinning `.wl-globe-sphere` (radial gloss + conic gradient) with two pseudo-element ellipses for the equator and meridian rings.
- **Celebración + Marcador chain** — the celebration overlay runs two CSS transition phases (`.active` → 2.4 s → `.shrinking` → 1.2 s) and then auto-calls `advance()`, dropping the show into the fullscreen scoreboard step. The corner widget's score-glow fires at the same moment the celebration panel begins to ease out.
- **Video** — `playVideo(url)` builds an autoplay embed URL and assigns it to the iframe `src`. `stopVideo()` clears the `src`. `clearOverlays()` calls `stopVideo()` on every transition, so audio cannot leak past the feature step. `toEmbedUrl()` normalises `youtu.be/<id>` and `?v=<id>` URLs.
- **Guest chyron** — the lower-third has no profile photo; it is a stack of three rows (top broadcast strip → name body with colour stripes → bottom broadcast strip). The `.guest-b` class on `#feature-guest` flips the stripe and pill colours for the second guest.
- **Web Audio SFX** — `osc()` + `noise()` helpers build oscillator and filtered-noise graphs on demand. Audio context unlocks on the first user gesture (`Empezar` click). The `sfx` object exposes `click`, `back`, `tick`, `beep`, `go`, `fanfare`, `shimmer`, `swoosh`, `whoosh`, `reveal`, `point(team)`, `celebrate(team)`, `victory`. Two looped pads (`startEnginePad` / `stopEnginePad` and `startEndLoopPad` / `stopEndLoopPad`) drive the trip bumper and the end-loop respectively; both are torn down by `clearOverlays()` and the mute toggle.
- **Particles** — `spawnConfetti`, `spawnSparkles`, `spawnStreaks` (radial speed lines), continuous ambient `dust`, and 5 large blurred floating `orb` divs.
- **Keyboard shortcuts** — Espacio (advance), Left-arrow / Backspace (back), M (mute toggle).

---

## License
Use it however you want for your class. No warranty.
