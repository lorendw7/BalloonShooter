# 🎈 BalloonShooter

A cozy, offline, single-player **3D rhythm-shooter** built with **Unity**. Pop balloons to the beat, paint the walls with every hit, and stumble into hidden surprises. Designed to be **easy to play, hard to put down** — generous timing, strong aim assist, zero punishment. No internet, no ads, no pressure.

---

## ✨ Features

### 🎵 Rhythm-First Gameplay (Not Just Shooting to Music)
Balloons don't simply float around — **the music composes the level**:

- **Beat Balloons** — spawn on the beat, glow brighter as their "sweet window" approaches. Pop them anytime, but popping *on the pulse* triggers a **Perfect**: bigger splat, richer sound layer, bonus score
- **Hold Balloons** — long balloons for sustained notes: keep the trigger held while the balloon stretches, release on the final beat. Feels like drawing a spray line across the wall
- **Chain Balloons** — short strings of 3–5 balloons arranged along a melody phrase. Pop them in order and the game *plays the melody back* through your hits
- **Echo Balloons** — call-and-response: the music plays a short pattern, then matching balloons appear. Repeat the rhythm by popping them — no memorization stress, the balloons light up as guides
- **Color Verse / Chorus** — the palette shifts with song sections: verses are calm single spawns, the chorus fills the air with clusters and confetti physics. You *feel* the song structure without reading a single note chart

### 🎯 Casual-Friendly Difficulty (Touch-First Design)
Built so a phone player never fights the aiming — and a couch gamepad player won't either once controller support lands:

- **Strong aim assist** — taps snap to the nearest poppable balloon; on gamepad (planned), the reticle magnetizes and a soft "snap" hops between targets with the right stick
- **One-Button Mode** — the game auto-cycles targets on the beat; you only press the trigger in rhythm. Pure rhythm, zero aiming — great for winding down
- **Generous timing windows** — Perfect / Good / Pop. Even "Pop" (any time) always counts and always paints. There is **no Miss penalty and no fail state**
- **Adaptive flow** — pop accuracy quietly tunes spawn density up or down. Doing great? More balloons, richer layers. Struggling? The song breathes and slows its spawns. Never a difficulty menu, never a wall
- **Slow-Bloom moments** — at phrase endings, time briefly softens (subtle slow-mo) so hold-releases and chain finales feel graceful instead of twitchy

### 🎨 Graffiti Paint System
- Every pop projects a **paint splat (URP Decal)** onto walls and floor — Perfect hits splash bigger and juicier
- Chain completions spray a **connected paint stroke** tracing the balloon path, like tagging a wall in one motion
- Hold balloons draw **continuous spray lines** while held
- The room slowly becomes your artwork; save the wall as a snapshot when the song ends
- Unlockable stencils (hearts, stars, cats, tags) triggered by Perfects and secrets

### 🥚 Easter Eggs & Surprises
- **Golden Balloon** — fireworks + score burst
- **Ghost Balloon** — nearly invisible; it hums quietly on the off-beat
- **Cat Balloon** — meows on pop, unlocks a paw stencil
- **Disco Balloon** — flips on a hidden music layer + party lights
- **Perfect full-phrase** — clearing a whole chain with all-Perfects rains paint across the scene
- A few secrets are deliberately undocumented. Keep popping. 👀

### 🧘 Relax-First Philosophy
- Fully **offline & local** — no account, no ads, no IAP
- **No fail, no punishment, no timers** in every mode
- **Zen Paint** mode: no score at all — just balloons, beats and paint
- Soft palettes, warm audio mixing, sessions that fit 3 minutes or an hour

---

## 🕹️ Game Modes

| Mode | Description |
|------|-------------|
| **Rhythm Pop** | Main mode — beat, hold, chain and echo balloons follow the song |
| **One-Button Groove** | Auto-aim on the beat; you only press in rhythm. Couch-perfect |
| **Zen Paint** | No score, no judgment — pure popping and painting |
| **Daily Wall** | One themed palette per day; finish the wall, save the artwork |
| **Egg Hunt** | Special balloons appear more often — find them all |

---

## 🎮 Controls

| Platform | Aim | Shoot / Hold | Switch Target | Stencil | Pause |
|----------|-----|--------------|---------------|---------|-------|
| **Android — Touch** (primary) | Tap directly (assisted) | Tap / press-hold | — | Palette swipe | ⏸ |
| PC — Mouse *(planned)* | Move mouse (assisted) | Left click / hold | — | Wheel | Esc |
| PC — Gamepad *(planned)* | Right stick (soft snap) | RT / R2 (press or hold) | Stick flick | LB / L1 | Start |

One shared input action map (Unity Input System), so PC and gamepad bolt on later without touching gameplay code. **Gamepad note:** everything will be playable with the right stick + one trigger — no twitch aiming ever required.

---

## 🛠️ Tech Stack

- **Engine**: Unity (3D, URP)
- **Input**: Unity Input System — unified maps for mouse / gamepad / touch
- **Rhythm sync**: `AudioSource.timeSamples` beat clock (sample-accurate, no drift); per-track beatmap assets (BPM, sections, chain phrases)
- **Aim assist**: screen-space target scoring + reticle magnetism; gamepad soft-snap
- **Paint**: URP Decal Projector, pooled decals; stroke decals for chains/holds
- **FX**: Particle System + Shader Graph (balloon pulse, splat edges, glow)
- **Save**: local JSON (unlocks, settings) + canvas snapshots
- **Localization**: Unity Localization — 简体中文 / English / 日本語 from day one (TMP with Noto Sans CJK fallbacks)
- **Platforms**: Android (primary target) / Windows + gamepad (post-demo) — local builds, fully offline

---

## 📁 Project Structure

```
Assets/
├── Scenes/            # Main, Menu, ZenPaint
├── Scripts/
│   ├── Core/          # Game loop, mode manager, save system
│   ├── Rhythm/        # Beat clock, beatmaps, judgment windows, adaptive flow
│   ├── Shooting/      # Raycast, aim assist, hold & chain logic, feedback
│   ├── Paint/         # Decal spawner, stroke painter, palette, snapshots
│   └── Eggs/          # Special balloons & unlocks
├── Prefabs/           # Balloon types, particles, decals, UI
├── Audio/             # Music (BPM-named, e.g. Sunset_100bpm.ogg), SFX layers
├── Materials/         # Balloon mats, splat & stroke textures, stencils
└── Settings/          # URP assets, input actions, beatmap data
```

---

## 🚀 Build & Run

1. Open with **Unity Hub** (Unity 6000.0.77f1 / Unity 6 LTS)
2. **Android** (primary): switch platform → IL2CPP + ARM64 → connect device → Build APK → install & play
3. **Windows** *(post-demo)*: `File → Build Settings → Windows` → Build & Run — gamepads via the shared input map

---

## 🗺️ Roadmap

Detailed milestones live in **[PLAN.md](PLAN.md)** (Android-first demo plan, in Chinese). Summary:

- [ ] **Demo (now)** — Android touch build: beat balloons, tap-to-pop with assist, paint splats, no-fail scoring, full 中/EN/日 UI
- [ ] **P1** — Windows build + gamepad support (reticle magnetism, soft snap, One-Button Groove, rumble)
- [ ] **P2** — full rhythm system: hold / chain / echo balloons, per-song beatmaps, adaptive flow
- [ ] **P3** — modes & secrets: Zen Paint, Daily Wall, Egg Hunt, stencils, PNG export of painted walls
- [ ] **P4** — custom music import with auto-BPM detection, colorblind-friendly palettes

---

## 📄 License

Personal project, for learning and fun. Feel free to explore — credit appreciated.
