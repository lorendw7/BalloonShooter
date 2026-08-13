# 🎈 BalloonShooter

A cozy, offline, single-player **3D balloon-shooting game** built with **Unity** — where shooting meets rhythm, and every hit paints the world. Pop balloons to the beat, cover the walls in graffiti, and hunt for hidden surprises. No internet, no ads, no pressure. Just you, the music, and a wall full of paint.


---

## ✨ Features

### 🎯 3D Shooting with a Unique Feel
- Balloons spawn, drift and rise in a stylized 3D space
- Raycast-based hit detection with squash-and-stretch pop animations
- Impact feedback trio: subtle screen shake + layered SFX + burst particles
- Balloon physics: wind drift, bobbing, chain reactions when balloons cluster

### 🎵 Rhythm-Game Core
- Balloons spawn, pulse and glow **in sync with the music's BPM**
- Hit on the beat → **Perfect** judgment: bonus score, richer sound layer, bigger paint splat
- Off-beat hits still count, but the game gently teaches you to *feel* the rhythm
- Beat timing driven by `AudioSource.timeSamples` (sample-accurate, no drift)
- Each track defines its own BPM, spawn pattern and color palette
- Combo system: consecutive Perfects build a multiplier and visual intensity

### 🎨 Graffiti / Spray-Paint System
- Every popped balloon **projects a paint splat (URP Decal)** onto walls and floor
- Splat size, shape and color depend on balloon type, hit accuracy and combo
- The longer you play, the more the room becomes **your own graffiti artwork**
- Canvas persistence: save your painted wall as a snapshot (PNG export planned)
- Unlockable stencil patterns: hearts, stars, cats, tags and more

### 🥚 Easter Eggs & Surprises
- Rare special balloons with hidden effects:
  - **Golden Balloon** — fireworks + score burst
  - **Ghost Balloon** — semi-invisible, listen for its faint hum
  - **Cat Balloon** — meows, unlocks a paw-print stencil
  - **Disco Balloon** — switches on a hidden music layer + party lighting
- Combo milestones trigger **paint rain** across the whole scene
- A few secrets are not listed here on purpose. Keep popping. 👀

### 🧘 Relax-First Design
- Fully **offline & local** — no account, no ads, no IAP
- Zen Mode: no score, no fail state, just balloons, beats and paint
- Session-friendly: pick up for 3 minutes or stay for an hour
- Soft color palettes and gentle audio mixing to lower stress, not raise it

---

## 🎮 Controls

| Platform | Aim | Shoot | Switch Stencil | Pause |
|----------|-----|-------|----------------|-------|
| PC — Mouse | Move mouse | Left click | Mouse wheel | Esc |
| PC — Gamepad | Right stick | RT / R2 | LB / L1 | Start |
| Android — Touch | Drag | Tap | Swipe on palette | ⏸ button |

All platforms share **one input action map** (Unity Input System). Same logic, same feel, everywhere.

---

## 🕹️ Game Modes

| Mode | Description |
|------|-------------|
| **Rhythm Pop** | Main mode — balloons follow the beat, chase Perfects and combos |
| **Zen Paint** | No score, no timer. Pure painting and popping |
| **Daily Wall** | One themed color palette per day; finish the wall, save the artwork |
| **Egg Hunt** | Special balloons appear more often — find them all |

---

## 🛠️ Tech Stack

- **Engine**: Unity (3D, URP)
- **Input**: Unity Input System — unified action maps for mouse / gamepad / touch
- **Rhythm sync**: `AudioSource.timeSamples`-based beat clock (sample-accurate)
- **Paint**: URP Decal Projector for splats; pooled decals for performance
- **FX**: Particle System + Shader Graph (balloon wobble, splat edges, glow)
- **Save**: Local JSON (unlocks, settings) + canvas snapshots
- **Platforms**: Windows / Android (local builds, fully offline)

---

## 📁 Project Structure

```
Assets/
├── Scenes/            # Main, Menu, ZenPaint
├── Scripts/
│   ├── Core/          # Game loop, mode manager, save system
│   ├── Shooting/      # Raycast, hit judge, feedback
│   ├── Rhythm/        # Beat clock, BPM config, judgment windows
│   ├── Paint/         # Decal spawner, palette, canvas snapshot
│   └── Eggs/          # Special balloon logic & unlocks
├── Prefabs/           # Balloons, particles, decals, UI
├── Audio/             # Music (named with BPM, e.g. Sunset_100bpm.ogg), SFX layers
├── Materials/         # Balloon mats, splat textures, stencils
└── Settings/          # URP assets, input actions
```

---

## 🚀 Build & Run

1. Open the project with **Unity Hub** (Unity 2022.3 LTS or newer recommended)
2. **Windows**: `File → Build Settings → Windows` → Build & Run
3. **Android**: switch platform to Android → connect device → Build APK → install & play
4. Gamepad: plug in any XInput-compatible controller — it just works

---

## 🗺️ Roadmap

- [ ] PNG export of your painted wall
- [ ] Custom music import with auto-BPM detection
- [ ] More stencils & hidden balloons
- [ ] Haptics on Android & gamepad rumble
- [ ] Colorblind-friendly palettes

---

## 📄 License

Personal project, for learning and fun. Feel free to explore the code — credit appreciated.
