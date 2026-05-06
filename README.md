# ¡Adivina la Profesion! — Spanish Class Game Show

A single-file, browser-based **TV game show** for a Spanish class, themed around guessing professions (jobs) in Spanish. Built as one self-contained `index.html` (HTML + CSS + JS) so the host (you) just opens it on a projector and plays.

The aesthetic is sleek, modern, dark-mode TV-show — neon blues, purples and pinks with glow effects, animated rings, gradient titles, and confetti.

---

## The idea

You are the host. Two student teams compete. The board shows **5 hidden words** (Spanish job names) laid out crossword-style. Each word starts as numbered placeholders. The host clicks a number, the room sees a hint (3 emojis), the team guesses, the host reveals the answer with a celebration, plays a short broadcast video, then loops back to the board to keep playing — until the host finishes the game and crowns a winning team.

Word **5** is special: it's the **final round**, so instead of emojis, the game randomly reveals **half the letters** of that word directly on the board. Teams have to figure out the rest.

---

## Flow

1. **Start screen** — neon title and "Start Game" button.
2. **Logo / intro** — the button plays an epic intro audio (placeholder) and shows an animated rotating logo for 3 seconds.
3. **Board** — scoreboard at the top (Team A vs Team B with ± buttons to adjust manually) and a crossword-style grid in the center with 5 numbered words.
4. **Hint modal** — clicking a number opens a modal with a "Show Hint" button. Clicking it shows 3 emojis representing the job.
5. **Reveal** — the host clicks "Reveal Answer". The Spanish word fills the screen with a gradient + sparkle celebration animation.
6. **Video broadcast** — a "Play Video" button opens a placeholder YouTube iframe (replace with your own clip).
7. **Back to board** — clicking "Back to Board" briefly replays the logo animation, then returns to the updated crossword.
8. **Final word (#5)** — clicking number 5 opens a special modal: no emojis, just a button that randomly reveals half of the word's letters on the board.
9. **Winner screen** — clicking "Finish Game" shows a full-screen, confetti-filled winner screen with a text input pre-filled with the leading team's name.

---

## How to use

### Open it
Just double-click `index.html` (or open it in any modern browser — Chrome, Edge, Firefox, Safari). No build step, no server, no dependencies.

### Customize for your class
Everything you'll want to change is at the top of the `<script>` block, marked with `PLACEHOLDER` comments:

#### 1. The 5 words and their emoji hints
```js
const WORDS = [
  { id: 1, word: "PROFESOR", emojis: ["📚", "🧑‍🏫", "🎓"] },          // PLACEHOLDER — replace
  { id: 2, word: "DOCTOR",   emojis: ["🩺", "💊", "🏥"] },             // PLACEHOLDER — replace
  { id: 3, word: "BOMBERO",  emojis: ["🔥", "🚒", "💧"] },             // PLACEHOLDER — replace
  { id: 4, word: "COCINERO", emojis: ["🍳", "🧑‍🍳", "🍴"] },           // PLACEHOLDER — replace
  { id: 5, word: "ARTISTA",  emojis: ["🎨", "🖌️", "🖼️"] },           // PLACEHOLDER — replace (FINAL — emojis unused)
];
```
Use UPPERCASE letters. Word 5's emojis are not displayed (final round shows half-letters instead), but keep the field so the array structure stays valid.

#### 2. The intro sound
Find this `<audio>` tag near the bottom of the body:
```html
<audio id="intro-sound" preload="auto">
  <source src="REPLACE_WITH_YOUR_INTRO_SOUND.mp3" type="audio/mpeg">
</audio>
```
Drop your own `intro.mp3` next to `index.html` and update the `src`.

#### 3. The broadcast video
Search for `PLACEHOLDER_VIDEO_URL` in the script:
```js
const PLACEHOLDER_VIDEO_URL = "https://www.youtube.com/embed/dQw4w9WgXcQ"; // REPLACE_WITH_YOUR_VIDEO_URL
```
Use the YouTube **embed** form (`https://www.youtube.com/embed/<VIDEO_ID>`).

---

## Host controls (during the show)

- **Score ± buttons** — adjust either team's score manually as you award/penalize points.
- **Number buttons (1–5)** — open the hint modal for that word.
- **Show Hint** — reveal the 3 emoji hints in the modal.
- **Reveal Answer** — close the modal, big-bang reveal the word full-screen, then offer "Play Video".
- **Back to Board** — replays the intro logo briefly, then drops you back into the updated board.
- **Reveal Half Letters** (word 5 only) — randomly fills in half the letters of the final word.
- **Finish Game** — go to the confetti winner screen.
- **Esc** — close any open modal.

---

## Tech notes

- **Single file**: zero dependencies, works fully offline (after you replace the audio src with a local file).
- **Responsive**: the layout collapses cleanly on smaller screens (laptop/tablet OK; phone usable but designed for projectors).
- **State is in-memory**: refreshing the page resets the game (the "Play Again" button after winner just reloads).

---

## Roadmap (ideas if you want to extend later)

- Save progress in `localStorage` so an accidental refresh doesn't reset the show.
- Per-word custom hint counts (4 emojis, mixed text+emoji, etc.).
- Sound effects for reveal / score changes.
- A "Wrong Answer" button on the modal that flashes red and locks that team out for 10 seconds.
- Real crossword intersections (currently the grid is row-based — each word in its own row with a number marker).

---

## License
Use it however you want for your class. No warranty, no fuss.
