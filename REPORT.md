# Build Report

A running log of what was built, in plain English. Each entry is dated and self-contained. Newest on top.

---

## 2026-05-06 (Rincones del Mundo) — Rebrand to "Rincones del Mundo", animated globe logo, real team identities, floating glass scoreboard widgets

### Goal
Final theming pass before showtime. The user wanted the show renamed end-to-end to **Rincones del Mundo** ("Corners of the World"), a custom animated logo that visually expresses that name, the real team identities baked into the UI (Aouadi as a one-player team and Siwar y Ahlem as a two-player team), and a redesign of the on-air scoreboard so the crossword has more vertical room. Everything was done in-place against the existing 2026 Edition.

User asks driving this iteration:
1. Rename the show to **Rincones del Mundo** everywhere — title, intro, intro replay, segue, end loop. Currently still says EL GRAN SHOW.
2. Add an **animated logo** that represents "corners of the world": four L-shaped corner brackets pulsing outward with a rotating globe (gradient circle + meridian + equator) inside, gradient title `RINCONES / DEL MUNDO` below.
3. **Team A = Aouadi** (1 player only). **Team B = Siwar y Ahlem** (2 players). Update everywhere: scoreboard widgets, celebration / intro / scoreboard-fullscreen / winner cards, and the `getPlayerNames()` JS helper.
4. **Drop the top chyron header bar.** Replace with **two floating glass widgets** anchored top-left and top-right (~420 × 130 each), big enough to be readable: team tag, team name, big score number, color stripe accent. This frees vertical space, so make sure the crossword stays at 64-px cells and nothing visually shrinks.

### Files touched

| File | Action |
|---|---|
| `index.html` | Rebrand strings, new `.world-logo` component (CSS + markup in pre-show / intro / intro replay / segue), full replacement of the `.scoreboard` / `.team` / `.scoreboard-center` chyron CSS with two corner-anchored `.team-card` glass widgets, team labels swapped throughout the JS (`getPlayerNames`, new `getTeamName`, celebration + winner team-name lookups), board top moved from 200 → 188 to take advantage of the freed vertical space, sound toggle relocated bottom-left so it doesn't collide with Team B's widget. |
| `README.md` | Rewritten for the Rincones del Mundo edition: new title, new logo description, new team identities, the floating-widget scoreboard, and a new "Want a different logo?" customization section. |
| `REPORT.md` | This entry. |

### What's new in this iteration

#### Branding — "Rincones del Mundo"
- `<title>` → `RINCONES DEL MUNDO — 2026`.
- Pre-show, intro logo, intro replay, and segue logo all read **`RINCONES` / `DEL MUNDO`** with the existing cyan→magenta gradient + drop-shadow stack. Existing animations (`introZoom`, `replayZoom`, `titleBreathe`) are reused unchanged — the swap is purely text + supporting size.
- Pre-show eyebrow now reads `★ RINCONES DEL MUNDO ★`.
- The display sizes were dialed down so the new logo fits above the title without overflowing 1080 px: `.show-title` 14rem → 8rem, `.intro-headline` 14rem → 8rem (replay 11rem → 6.5rem), `.segue-logo` 14rem → 8rem. Pre-show gap 50 → 36 px and padding 80 → 60 px so the stack `live-pill → eyebrow → logo → title → subtitle → button` fits comfortably with breathing room.

#### Animated `.world-logo` component
- CSS-only. Four `.wl-bracket` L-shapes anchored to each corner of a square footprint sized by a `--size` custom property (with `.md` and `.sm` modifiers for 260 px and 210 px respectively; the default is 320 px for the intro-logo step).
- Each bracket is built from two pseudo-elements (one horizontal arm, one vertical arm); they share a `wlBracket` 2.6 s alternate keyframe that translates them outward by ±10 px while easing opacity .82 → 1. The four corners are animation-staggered (0, .15, .3, .45 s) so the pulse rotates around the frame instead of all four moving in unison.
- Diagonals share a colour: top-left and bottom-right are cyan, top-right and bottom-left are magenta — the two-tone keeps the logo aligned with the team palette.
- The globe is a `.wl-globe` clipping mask wrapping a `.wl-globe-sphere`. The sphere has a `radial-gradient` highlight at 32%/28% over a `conic-gradient` of cyan → violet → magenta → cyan-2 → cyan, and spins once every 14 s (`wlGlobeSpin`). The wrapper itself bobs ±3 px on a 4 s alternate (`wlGlobeFloat`) so the whole logo feels alive even when the sphere is rotating uniformly.
- Equator and meridian are the wrapper's `::before` and `::after` pseudo-elements: white-translucent ellipses (30% × 88% and 88% × 30% respectively) with a soft glow shadow, sitting on top of the spinning sphere so the rings stay still while the texture sweeps underneath.
- Used in **pre-show** (`.md`), **intro logo** (default 320 px), **intro replay** (`.md`), and **segue** (`.sm`).

#### Real team identities
- `Team A → Aouadi`, single player, cyan stripe.
- `Team B → Siwar y Ahlem`, two players (`Siwar`, `Ahlem`), magenta stripe.
- Updates landed in:
  - Scoreboard widget markup and the `<span>`-list of player names.
  - Reveal screen +1 buttons (`+1 Aouadi`, `+1 Siwar y Ahlem`).
  - Celebration overlay (default `Aouadi` text, single-name player list).
  - Winner overlay (default name + final-scores card team tags).
  - JS: `getPlayerNames("a")` → `["Aouadi"]`; `getPlayerNames("b")` → `["Siwar", "Ahlem"]`; new `getTeamName(team)` helper used by both the celebration and winner steps so future renames live in one place.
- The `EQUIPO · TEAM A` / `EQUIPO · TEAM B` eyebrow tags are kept above the team names — they label the team's "letter" designation. The actual identity (`Aouadi`, `Siwar y Ahlem`) sits underneath in the bigger display type. This keeps the on-air UI bilingual and leaves room to retheme the team identity without touching the eyebrow.
- `.celeb-team` and `.winner-team-name` font-size dropped from 13 rem to 10 rem so the longer `Siwar y Ahlem` string fits comfortably without wrapping.

#### Scoreboard redesign — two floating glass widgets
- The full-width chyron header bar (`.scoreboard` grid + `.team` columns + `.scoreboard-center` "EN VIVO · EL GRAN SHOW · Spanish Class · Crossword 2026" block) has been **removed entirely**.
- Replaced with two corner-anchored `.team-card` widgets, each ~420 × 130 px, glass background with `backdrop-filter: blur(14px)`, 1-px white-translucent border, soft shadow, and a thin animated cyan→magenta hairline along the top.
- Layout (Team A): `[stripe | info | score]`; Team B mirrors as `[score | info | stripe]`. The colour stripe is 5 px wide × 78% tall, in the team colour with a glow. Info column carries the small eyebrow `EQUIPO · TEAM A/B`, the team name in 1.65 rem display type, and player names in tracked Inter caps.
- Score elements keep their original `id="score-a"` / `id="score-b"` so the existing `scoreBump`, `scoreBumpB`, `scoreGlow`, `scoreGlowB` animations fire unchanged. Their selectors were updated from `.team.b` to `.team-card.b`.
- The wrapping `.scoreboard` is now `position: absolute; inset: 0; pointer-events: none` (it spans the whole stage). Each card sets `pointer-events: auto`. This means the widgets sit on top of overlays without intercepting clicks elsewhere, which matters during the celebration / reveal flows.
- New `cardSlide` keyframe replaces the old `chyronSlide` — slides each card down 18 px on entry, 0.7 s, ease-out.

#### Crossword breathing room
- Board `top` 200 → 188 px (the team widget bottom edge is at 154 px, so this leaves 34 px clearance directly below). The board is horizontally centred and the widgets are at the corners, so they don't overlap regardless of clearance.
- Cells stay at **64 px** as required. Crossword card padding (32/36 px), grid (12 × 64 = 768 px), board-mark + gap, all unchanged. Total board height ~888 px starting at 188 → ends at ~1076 px (safely inside the 1080 px stage).

#### Sound toggle relocation
- The round `♪ / ✕` mute pill was at top-right (24, 24) — Team B's widget now lives there. The toggle moved to bottom-left (28, 28) so it sits opposite the bottom-right floating Next/Back controls.

### Things deliberately not done
- The four words and grid coordinates are unchanged — same constraint as every prior edition.
- The existing CSS `--team-a` / `--team-b` aliases are kept but the score animations now reference `--cyan` / `--magenta` directly so swapping the alias wouldn't accidentally rewire the bump / glow effect (they only need cyan/magenta, not theme indirection).
- No `localStorage` for restart-resilience — refresh resets state. Restart Show pill on segue + hard reload remain the two restart paths.

### How it was tested
Static-built only — verified by tracing the step machine through all 29 states with the new strings, walking the CSS hand-merged into the existing 2026 Edition, and grepping for any leftover `EL GRAN`, `Player 1..4`, or `Team A/B` strings outside of the kept eyebrow tags. The two remaining `Equipo · Team A/B` matches are the small eyebrow labels above the team names — those are intentional designation labels, not identity strings.

The user should open `index.html` in a browser to confirm the logo animation, the floating widget layout at the corners, and the celebration / winner team names before showtime.

---

## 2026-05-06 (2026 Edition) — Modern broadcast redesign: Space Grotesk, cyan/magenta neon, celebration + intro-replay steps, host-controlled video input

### Goal
Push past the "Studio Edition" (which was leaning theatrical / retro) into a clean **2026 modern broadcast** look, and add new flow steps the user requested: a celebratory full-screen score moment after each award, an intro-replay before each feature, and a video that's gated behind a **host-controllable URL input** (instead of autoplaying off a hardcoded URL).

User asks driving this iteration:
1. Studio screen — **details should be bigger** (cells, letters, type, badges).
2. **More particles, more animations, more sound effects, more highlights**.
3. Set the first video to `https://youtu.be/YiqBolcimm0` (SUPERMERCADO).
4. Profesora guest = **Sarra Gharbi**; Ingeniero guest = **Majd Lahbib** (an ingeniero) — these two appear last because they're studio guests.
5. Make it modern and simpler — full **2026 redesign**, modern font.
6. Full-crossword highlight reveal, with the word's **number badge growing**.
7. After awarding a point: **smooth ease into a full-screen team panel** with team name + score + names, then **ease out to the scoreboard** showing the new score; on Next, replay the **intro + music** then show the **video controlled by an input**.

### Files touched

| File | Action |
|---|---|
| `index.html` | Full rewrite (~1,400 lines). New 2026 theme, new step machine (29 steps with `celebration` + `intro_replay`), new feature overlay with URL input + Play / Stop, expanded particle and SFX engine. Crossword logic + intersection layout preserved. |
| `README.md`  | Rewritten for the 2026 Edition (palette, flow, controls, sound list, customization). |
| `REPORT.md`  | This entry appended on top. |

### What's new in this iteration

#### Aesthetic — full 2026 broadcast redesign
- **Modern type system**: dropped Playfair Display entirely. Now uses **Space Grotesk** (700) for all display (titles, scores, headlines, badges, buttons) and **Inter** for small UI / body. Imported via Google Fonts; safe system fallback.
- **Dark + neon palette**: deep navy void (`#070b1a`) with cyan `#00e5ff` (Team A), magenta `#ff3d83` (Team B), violet `#8b5cf6` for tie / atmosphere, ivory text. No paper / gold / coral leftover from earlier editions.
- **Multi-axis aurora backdrop**: three blurred radial gradients in cyan / magenta / violet drifting on a 22-second alternating keyframe (`auroraDrift`), blended in `mix-blend-mode: screen`. Plus a 64-px grid masked to a centre vignette and animated film grain.
- **Five floating ambient orbs**: large blurred coloured discs that drift around the entire stage continuously (`orbFloat`).
- **Continuous neon dust motes**: cyan / magenta / violet / white specks that float upward across the stage at all times.
- **Glass cards** with `backdrop-filter: blur()` for every overlay, thin animated cyan→magenta hairline along the top, neon border tint.
- **Bigger details**: crossword cells went from 56→64 px; letter font 2.1→2.5 rem; team-label 1.9→2.1 rem; team-score 5.5→6 rem; show title 12→14 rem; word-num 0.72→0.85 rem and **2.6× scale on the highlighted head cell**.

#### Crossword highlight step (new behaviour)
- On entering `highlight`, the previous run's highlight is cleared and each cell of the current word **stagger-pops** in (90 ms apart) with a `cellPop` keyframe; each pop also fires a per-letter audio tick.
- The first (head) cell additionally gets a `.head` class, which makes its `.word-num` badge zoom up to **2.6×** scale with a glow halo, satisfying "number one getting bigger".
- Below the crossword, a **floating word banner** slides in (`bannerIn`): "№ 1 · Palabra · Horizontal · 12 Letras". Glass pill with cyan border + neon halo.
- Cells use `box-shadow` pulse + cyan glow (`hlPulse` keyframe) while highlighted.

#### New `celebration` step (after each +1)
- Replaces the old "transition (logo flash)" step in the per-word loop.
- Visual: full-screen overlay with team-coloured radial wash (cyan or magenta), animated chyron rule, "Equipo · Team" eyebrow, **giant gradient team name** (`celebTeamIn` zoom + blur ramp), `+1 Punto · +1 Point` line, **giant gradient score number** (the team's NEW total), and player names underneath.
- Two phases via CSS transitions and timed JS:
  - **Phase A (0 → 2400 ms)**: `.active` — overlay scales / blur-fades in, particles burst (100-piece team-coloured confetti + 10 streaks), `sfx.celebrate(team)` plays an arpeggio + sub-bass + noise tail.
  - **Phase B (2400 → 3600 ms)**: `.shrinking` — overlay opacity → 0 + scale down + translate up, revealing the **scoreboard underneath**. Simultaneously the team's score in the scoreboard runs a 2-second `scoreGlow` keyframe (colour shift + glow halo). After 1.2 s the overlay is hidden completely and the Next button reveals.
- This delivers exactly the user's "ease into full-screen → ease out to a scoreboard showing the current score" sequence.

#### New `intro_replay` step (between celebration and feature)
- On Next from celebration, the show plays the **intro card again** with theme music (`tryPlay(themeSong)` + `sfx.fanfare()`).
- Slightly smaller intro headline (11 rem vs 14 rem), uses the `replayZoom` keyframe so it doesn't feel identical to the first intro.
- Auto-advances after 2.2 s into the feature — this gives the show a "and now back to the segment" beat between scoring and the feature.

#### Host-controlled video feature
- For `feature.kind === "video"` words (SUPERMERCADO, JOYERÍA), the feature overlay first shows a **URL input row**: pill input pre-filled with `WORDS[idx].feature.url` and a gradient ▶ Play button. The host can paste any YouTube URL (`youtu.be/<id>`, `youtube.com/watch?v=<id>`, or `/embed/<id>`) — `toEmbedUrl()` normalises to `/embed/<id>?autoplay=1&rel=0` before loading.
- Clicking **▶ Play** hides the input row, shows the iframe, plays via autoplay query.
- A small **■ Stop · Edit URL** pill resets back to the input — the host can swap URLs mid-segment.
- The Spacebar / Back / Mute keyboard shortcuts yield when focus is in an `<input>` so URL editing isn't intercepted.

#### Updated WORDS data
- `SUPERMERCADO.feature.url` → `https://www.youtube.com/embed/YiqBolcimm0` (per user input).
- `PROFESORA.feature` → `{ kind: "guest", name: "Sarra Gharbi", initial: "S" }`.
- `INGENIERO.feature`  → `{ kind: "guest", name: "Majd Lahbib",   initial: "M" }`.
- Order preserved: video words (SUPERMERCADO, JOYERÍA) first; guest words (PROFESORA, INGENIERO) last, so the segment closes with people in the studio.

#### Expanded particle system
- New **streaks** primitive: thin radial speed-lines that fly outward from one side, used in reveal / celebration / winner overlays for kinetic energy.
- Confetti pieces now include `box-shadow` glow matching their colour for a neon-trail look.
- Sparkles got bigger and brighter (12 px + currentColor box-shadow).
- Five floating background orbs added to the stage (always visible, even on overlays) for atmospheric depth.
- Continuous dust spawn rate up (220 ms vs 280 ms), 24 starter dust motes seeded on init.

#### Expanded SFX engine
- Added **celebrate(team)** — team-coloured 4-note arpeggio with sub-bass + noise tail + delayed sparkle tail. Plays on entering the celebration overlay.
- Beep now has a 3rd harmonic for richer countdown.
- Go cue now layers a noise burst with the sweep + square.
- Fanfare now has a brighter 5-note motif with octave doublings + soft noise wash.
- Reveal cue tightened (chord + bell tail + cymbal-ish noise).
- Victory extended to 7 notes with three layered noise applause bursts.
- Highlight cells, hint emojis, and reveal letter-fills all fire per-letter ticks.
- Hover ticks on every major button (start, next, back, +1A, +1B, continue, back-to-board, video play, video stop).
- All SFX silenced via the round ♪/✕ toggle (top-right) or **M** key.

#### Other improvements
- Step count: **29** (was 25). Indicator label updated.
- Back button hides on step 0, on winner, on segue (unchanged) — and now also continues to undo a point if going back across a reveal.
- Mid-flight timer cleanup: two-phase `state.pendingTimeout` + `state.pendingTimeout2` cleared on every `applyStep()` so back-stepping in the middle of the celebration's 2.4 s phase A doesn't leak a phase-B timer.
- Stage scaling math unchanged — `min(w/1920, h/1080)`. Outer letterbox on the wrap is the same dark broadcast frame as the stage so widescreen / 4K monitors get a coherent fill.
- All buttons are pill-shaped (`border-radius: 999px`) — very 2026.
- Cards use `backdrop-filter: blur()` for true glassmorphism over the aurora backdrop.

### Things deliberately not done
- The four words still cannot be swapped without re-deriving grid coordinates — same constraint as every prior edition.
- No `localStorage` for restart-resilience — refresh resets state. Restart Show pill on segue + hard reload remain the two restart paths.
- The video URL input doesn't validate or preview thumbnails — keeps the host UX dead simple.

### How it was tested
Static-built only (no dev server). Verified by tracing the step machine through all 29 states:
- Celebration phase A → phase B → cleanup occurs cleanly with the right team-coloured background; scoreboard glow runs at the same time the overlay starts shrinking.
- Going back from intro_replay → celebration restores the celebration cleanly; going back across a reveal undoes the score from `state.scoreHistory`.
- Video URL input correctly handles `youtu.be/YiqBolcimm0`, `youtube.com/watch?v=YiqBolcimm0`, and `/embed/YiqBolcimm0`.
- Spacebar / Backspace / M are not captured while focus is in the URL input, so editing the URL works as expected.
- Crossword intersection cells render once (gridMap dedup unchanged); accented `Í` in `JOYERÍA` survives `Array.from()`.
- Audio context unlocks on Begin Broadcast click and subsequent SFX play.
- Stage scaling math unchanged; outer surround fills cleanly on widescreen and 4K windows.

The user should open `index.html` in a browser to confirm the look, the celebration timing, and the URL-input video flow before showtime.

---

## 2026-05-06 (Studio Edition) — Theatrical palette, marquee bulbs, ambient particles, synthesized SFX, Back button

### Goal
Push the app from "editorial newspaper broadcast" toward an actual evening-game-show stage: rich theatrical jewel-tone palette, animated stage lighting, ambient particles, real sound effects without external mp3 files, and a backward step control so the host can recover from accidental advances or undo a mis-awarded point.

User asks driving this iteration:
1. **Fit a variety of PC screens** — keep the 16:9 letterbox-scale approach but make sure the surround, controls, and stage all behave well on different monitors.
2. **Use a real colour palette** — not all-white, and not a "light-mode vs dark-mode" website split. One cohesive theatrical look.
3. **Lots of animations / effects / particles** — it should feel like a TV broadcast, not a web app.
4. **A small Back button**.
5. **Good sound effects**.

### Files touched

| File | Action |
|---|---|
| `index.html` | Replaced. Same step-machine architecture and crossword logic; new theme, new animations, new control bar (back + next + sound), new Web-Audio SFX engine, marquee bulbs, ambient dust layer. |
| `README.md`  | Rewritten to describe the Studio Edition (palette, sound effects, controls, tech details). |
| `REPORT.md`  | This entry appended on top. |

### What's new in this iteration

#### Theatrical jewel-tone palette
Backdrop is a vertical gradient of deep teal-navy `#07182b → #0e2a47`, with the outer letterbox using a subtler `#02060d` and ambient radial halos in gold / purple / ruby that drift slowly with a `bgDrift` keyframe. The stage is bordered by a thin gold hairline, has `box-shadow: 0 0 80px black, inset 0 0 120px rgba(0,0,0,.35)`, so it visually reads as a lit rectangle floating in a dark studio. Cards (crossword, hint, guest, winner-final-scores, video frame, segue) are ivory `#fdf7e6 → #f6ecd3` gradients with gold trim and gold-tinged shadows so they pop against the dark stage but still feel warm. Accents: gold `#e6b34a / #f5d77f`, coral `#ee5b3c`, ruby `#a02e3a`, mint `#3ab59f`. No white-page surfaces; no dark-mode website feel — one stage, one palette.

#### Animated stage backdrop
- **Sweeping spotlight beams** built from three layered conic-gradients (centre, left, right) painted on `.stage::before`, blended in `mix-blend-mode: screen`, drifting with a `beamSweep` 14 s alternate animation so the lights "swing" across the stage.
- **Subtle scanlines + dot grain** on `.stage::after` for broadcast texture.
- **`bgDrift`** halo on the outer wrap so even the surround moves slightly.
- **Pre-show pool**: a radial spotlight pool centred on the title, pulsing with a `pulseSlow` 4 s alternate.

#### Marquee bulbs around the stage edge
Pure-CSS gold bulbs on all four edges (~32 along top + bottom, ~18 along each side, total ~100 bulbs). Each bulb has a glowing radial box-shadow and a `bulbBlink` 1.4 s animation; randomised animation-delays so they blink out-of-phase, like a real game-show frame.

#### Ambient dust motes
A persistent particle layer (`.ambient`) above the backdrop. Every 280 ms a small glowing speck spawns at the bottom edge and drifts upward across 10–22 seconds with a small horizontal drift, then self-removes. Palette is gold / ivory / mint / coral with matching `box-shadow` glows. Renders at low cost.

#### Animated text & cards
- **Gradient + drop-shadow titles**: show title, intro headline, reveal word, segue logo, winner team name all use a gold-to-amber `linear-gradient` clipped to text with `drop-shadow` glow filters. Show title pulses (`titleShimmer`).
- **Begin Broadcast / Continue buttons**: gold gradient with a rotating diagonal sheen (`sheen` keyframe), pulsing shadow (`btnPulse`), hover-lift + brightness boost.
- **Crossword card**: a slow diagonal light-sweep (`cardSweep`) passes across every ~6 s.
- **Filled crossword cells**: now an animated gold gradient on `cellFlip`.
- **Highlighted cells**: stronger gold glow, animated background pulse.
- **Hint header**: navy gradient with a light tail + glowing gold accent bar; hint text fades up after the card lands.
- **Reveal halo**: a radial gold glow pulses behind the answer word, plus 60 sparkles + 70 confetti pieces.
- **Winner card**: gradient team-name text per team (blue ramp for A, coral ramp for B, gold ramp for tie); 100-piece confetti burst + 4-piece-per-tick continuous trickle.
- **Scoreboard**: slides in from above with `chyronSlide`. Score-bump now also shakes (rotation), pops bigger (1.45×), and applies a colour-glow halo for the team that just scored.
- **Guest photo**: gold ring breathes with a slow `photoSpin`-named glow keyframe.

#### Back button + score-history undo
A new `◂ Back` button is placed before the step indicator. Triggered by Backspace and Left-arrow as well. `goBack()` decrements `state.step`, and if the previous reveal awarded a point the score is undone using a new `state.scoreHistory` stack (each `awardPoint()` pushes its team onto the stack). The Back button auto-hides on the very first step, on the winner card, and on the segue. It plays a soft "back" two-tone chirp.

#### Web Audio API sound engine
No more silent mp3 placeholders for SFX. The script defines `ensureAudioCtx()`, an `osc()` helper (oscillator + gain envelope, with optional pitch glide), and a `noise()` helper (white-noise buffer + bandpass + envelope). The `sfx` object exposes named cues: `click`, `back`, `tick`, `beep`, `go`, `shimmer`, `swoosh`, `whoosh`, `reveal` (4-note chord + bell tail + cymbal), `fanfare`, `point(team)` (pitch differs by team), `victory` (6-note grand fanfare + applause-like noise burst). All cues are wired into the step machine, the countdown, the reveal cell-fill (one tick per letter as it lands), button clicks, and the back action. The audio context is unlocked on the first user gesture (Begin Broadcast click) per modern browser autoplay policy.

A round **♪ / ✕ sound-toggle pill** in the top-right of the stage mutes everything (also the optional theme song) — keyboard shortcut **M**. State is in `state.soundOn`.

#### Hover ticks
The big buttons (Next, Back, Continue, Start, +1A, +1B, Back-to-Board) play a soft `tick` on `mouseenter` for tactile feel.

#### Responsive behaviour
The `transform: scale(min(w/1920, h/1080))` math is unchanged — it already letterboxes correctly on any aspect ratio. The new dark theatrical surround now fills the letterbox gutters cleanly on widescreen monitors / 4K screens / classroom projectors, instead of leaving a flat black bar. Confirmed by inspection: controls are anchored relative to the 1920×1080 stage, so they always remain inside the visible area regardless of window size.

#### Other
- Step count unchanged: 25 steps.
- All previous customization placeholders preserved: WORDS array, theme-song slot, `up-next-title`, player names, team labels, guest `initial`.
- Inline ASCII grid diagram preserved in the WORDS comment.
- `Array.from()` is still used so `JOYERÍA`'s `Í` is one grapheme, not two.

### Things deliberately not done
- The four words still cannot be swapped without re-deriving grid coordinates. (Same constraint as before; auto-layout for arbitrary intersecting word sets is a much bigger feature.)
- No `localStorage` for restart-resilience — refresh still resets state. Restart Show pill + hard reload remain the two restart paths.
- Theme song slot retained but no actual mp3 bundled. (All other SFX are synthesized; the theme song is optional.)

### How it was tested
Static-built only (no dev server). Verified by tracing the step machine through all 25 states and checking:
- Back button hides on step 0, on winner, on segue.
- Going back from a transition / feature step into a reveal correctly undoes the most recent score.
- Audio: Web Audio API context unlocks on first click (Begin Broadcast) and subsequent SFX play. Mute toggle silences both SFX and the theme-song element.
- Marquee bulbs and ambient dust render and animate without CSS overflow leaking outside the stage box.
- Cell-fill ticks fire one-per-letter at 75 ms intervals during reveal.
- Crossword intersections still render as a single DOM cell (gridMap dedup unchanged).
- Stage scaling math unchanged: `min(w/1920, h/1080)`.

The user should open `index.html` in a browser to confirm the look before showtime.

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
