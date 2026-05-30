# 🍉 Fruit Ninja AR — Hand Gesture Edition

A browser-based AR remake of Fruit Ninja. **No mouse, no controller — just your hand and a webcam.** A virtual katana follows your palm; swipe through the air to slice fruit on your screen. Single self-contained HTML file. No build step.

🎮 **[Play it live →](https://fruit-ninja-ar.vercel.app/))** *(enable GitHub Pages in repo settings to activate)*

---

## ⚔️ Features

### Hand tracking
- **MediaPipe Hands** pulls 21 landmarks per hand at ~30 fps from your webcam
- **One-Euro Filter** smooths jitter while staying responsive during fast swings
- **60 fps render-side chase** so the katana feels fluid even at 30 fps tracking input
- **Dual-katana mode** — turn it on in Settings to wield two swords at once. Right hand gets a warm gold-trim katana, left hand gets a cool silver-blue one. Both are tracked independently; either can slice; swipe combos pool across hands.

### Gameplay
- **Three modes** — Classic (3 lives), Arcade (60s timed), Zen (90s, no bombs)
- **Swipe combos** — slice 2+ fruits in one continuous motion for Double / Triple / Frenzy / Insane multipliers
- **Dynamic difficulty** — 10 progression tiers with endless mode past level 10
- **Power-up fruits** — Freeze Banana (cyan, slows time 5s), Bonus Star (gold, +life/+time/+score), Pomegranate (bursts into 8 mini-fruits)
- **Bomb mechanics** — costs a life in Classic, time penalty in Arcade
- **Near-miss detection** — bomb whoosh-by triggers a CLOSE! callout and screen flash

### Polish
- Detailed **katana anatomy** drawn entirely in Canvas — tsuka (diamond-wrap handle), tsuba (guard), habaki (collar), curved blade with bo-hi (blood groove), hamon (temper line), kissaki (tip)
- **Speed-reactive blade** — motion-blur ghost copies above 400 px/s, glow above 600 px/s
- **Slash trail** with adaptive width and fading alpha
- **Slice afterimages** — each cut leaves a brief glowing line where the blade passed
- **Pre-game 3-2-1 countdown** so you have time to position your hands
- **Web Audio synthesized SFX** + ambient drone music (everything is generated, no audio files)
- **Hit-freeze** on slice, **sustained slo-mo** on Frenzy+, **canvas combo-zoom** for big swings

### Persistence
- Per-mode leaderboards (top 5 each)
- 10 achievements with animated popups
- Career stats (total runs, fruits, bombs, best level, best score, best combo)
- Settings (sound, music, perf mode, dual katana)
- All stored in `localStorage`, with in-memory fallback when storage is blocked

---

## 🎮 How to play

1. Open the live URL in **Chrome, Edge, or Firefox** (Safari works but has slower hand tracking)
2. Click **ENABLE CAMERA** and allow access — the feed is processed entirely in your browser, nothing is recorded or uploaded
3. Hold your hand up to the camera — a **katana appears in your palm**
4. Point your **index finger** to aim the blade
5. **Swipe** through fruit to slice. Bombs end your run (or cost a life).
6. Cut **multiple fruits in one swipe** for combo bonuses
7. Toggle **DUAL KATANA MODE** in Settings to use both hands

### Tips
- Stand back ~1 meter from the camera so MediaPipe can see your full hand
- Good lighting helps tracking accuracy a lot
- The blade extends past your fingertip — judge the tip, not your finger
- In dual mode, you can sustain a Frenzy by alternating slashes between hands

---

## 🛠️ Tech stack

| Component | Choice |
|---|---|
| Hand tracking | [MediaPipe Hands](https://google.github.io/mediapipe/solutions/hands.html) (CDN) |
| Rendering | HTML5 Canvas 2D, procedural — no sprite assets |
| Audio | Web Audio API, all sounds synthesized at runtime |
| Persistence | `localStorage` with in-memory fallback |
| Fonts | Google Fonts (Bangers, Luckiest Guy, Fredoka) |
| Build | None — single `index.html` file, ~5,300 lines |

---

## 💻 Running locally

The webcam requires a **secure context** (HTTPS or `localhost`). Opening `index.html` directly via `file://` will not work — Chrome blocks `getUserMedia` from local files.

**Option A — Python (probably already on your machine):**
```bash
git clone https://github.com/pranavmudaliar-28/fruit-ninja-ar.git
cd fruit-ninja-ar
python -m http.server 8000
# then open http://localhost:8000/ in your browser
```

**Option B — Node:**
```bash
npx serve .
```

**Option C — VS Code Live Server extension** (right-click `index.html` → "Open with Live Server")

---

## 🚀 Deploying to GitHub Pages

This is the easiest way to host the game so anyone with the URL can play:

1. Push this repo to GitHub (you've done that — this is the repo)
2. In the GitHub repo, go to **Settings → Pages**
3. Under **Build and deployment**:
   - Source: **Deploy from a branch**
   - Branch: **main**
   - Folder: **/ (root)**
4. Click **Save**
5. Wait ~30 seconds, then open `https://pranavmudaliar-28.github.io/fruit-ninja-ar/`

GitHub Pages serves over HTTPS automatically, so the webcam works. No server config, no payment, no domain setup.

---

## ⚙️ Project structure

```
fruit-ninja-ar/
├── index.html        ← the entire game (HTML + CSS + JS, ~5,300 lines)
├── README.md         ← this file
├── .gitignore
└── LICENSE
```

The whole game is in `index.html`. Open it in any editor to inspect or modify. Major sections inside:

| Section | What it does |
|---|---|
| `<style>` | All CSS — game UI, overlays, animations, responsive media queries |
| `CONFIG = {...}` | Tunable constants — spawn rates, difficulty scaling, swipe thresholds |
| `class OneEuroFilter` | Adaptive low-pass filter for hand jitter |
| `class HandState` | Bundles per-hand state (raw, render, trail, filters, blade speed) |
| `class SoundManager` | All Web Audio synthesized SFX + ambient music |
| `class HandTracker` | Wraps MediaPipe Hands, emits normalized hand data |
| `class Trail` | Slash arc geometry + collision segments |
| `class Fruit` / `class Bomb` | Spawned objects with physics, slicing, trails, particles |
| `class Katana` | Drawing pipeline for the on-screen sword (with `warm` / `cool` palettes) |
| `class Game` | Main loop, state machine, scoring, achievements, persistence |

---

## 🪪 License

MIT — see [LICENSE](LICENSE). Built with [Claude](https://claude.ai) + MediaPipe.

---

## 🔧 Known limitations

- Camera permission is required — the game cannot work without webcam access
- Performance varies with CPU — turn on **Performance Mode** in Settings for older machines
- MediaPipe occasionally swaps left/right handedness on a frame; the game routes by handedness label, so a stable two-handed pose works best
- Safari's hand-tracking pipeline is noticeably slower than Chromium-based browsers
