# Metal Shatter Arena

A single-file, browser-based top-down neon mech arena shooter with destructible voxel enemies and terrain. Offline-ready PWA.

## Features

- Title screen (PLAY, HIGH SCORE, SETTINGS); audio unlocks on first tap
- Persistence via `localStorage` with memory fallback (high score, best wave, volume, mute, quality, screen shake, fullscreen pref)
- Settings: master volume, mute (M), low/high quality, screen-shake toggle, fullscreen (F)
- Auto quality: LOW/MED/HIGH (DPR 1 / 1.5 / 2); drop below 50fps, raise after 5s at 58fps+
- Boss every 5 waves, wave-clear banner, combo multiplier, hit-flash, damage vignette
- Pause on tab hide / blur; mute audio and cancel plasma loop; resume cleanly
- Screen Wake Lock, fullscreen, landscape hint on mobile, safe-area insets
- Gamepad vibration + `navigator.vibrate` (guarded)
- Offline play via service worker (navigation network-first, assets stale-while-revalidate)

## Controls

| Action     | Keyboard / Mouse              | Touch                | Gamepad             |
| ---------- | ----------------------------- | -------------------- | ------------------- |
| Move       | WASD / Arrows                 | Left stick           | Left stick          |
| Aim / Fire | Mouse aim + click/hold, Space | Right stick          | Right stick, A / RT |
| Shield     | Hold Shift                    | SHIELD button        | LB / RB             |
| Autocannon | 1                             | Weapon HUD           | —                   |
| Plasma     | 2                             | Weapon HUD           | —                   |
| Rocket     | 3                             | Weapon HUD           | —                   |
| Pause      | Esc / P                       | II button            | —                   |
| Mute       | M                             | Settings             | —                   |
| Fullscreen | F                             | FS button / Settings | —                   |

Shield is a 90° arc toward aim. Heat cools while not firing; overheat locks weapons until heat drops. Shield regenerates only while not held.

## Combat

- Enemy hulls use an offscreen Armor → Hull → Core voxel mask, erased in local space so holes stay aligned while units rotate
- Autocannon punches a deep tunnel, plasma burns a wide disc, rockets erase a radial blast
- Combo multiplies kill score; bosses spawn every 5 waves
- Heat meter, directional shield, weapon HUD, camera shake / explosion zoom / velocity lag

## Tech notes

- Pure HTML/CSS/JS, `<canvas>` 2D, no libraries or CDNs
- Destructible terrain via a per-pixel solidity buffer (`Uint8Array`)
- Enemy navigation: BFS flow-field over a coarse grid
- Object pools for particles, debris, projectiles, damage popups, and 8-voice audio
- Top-down only: no gravity; radial drag `exp(-k*dt)` on FX; all motion `v*dt`
- Enemy shots: 4-5px core + cached glow, 3-point trail, 0.25s charge ring + muzzle flash
- Death FX: shockwave, radial debris, hit-stop ≤60ms, size-scaled shake; low-HP smoke follows enemy velocity
- Spatial audio voice pool (polyphony 8); skip panner past `MAX_RANGE`; plasma loop stops on pause/switch/overheat/death/restart
- Player IK: hips from torso rotation; planted step targets; lerp to rest when idle
- Headless self-test: `?test=1` (not run on production load)
- Perf: `?perf=1` overlay, `?bench=1` 30s seeded report (`window.__MSA_BENCH`), `?stress=1` 60s harness
- Fixed timestep 1/60 (max 3 steps); flow-field gated 100ms / player-cell; nav rebuild budgeted 150 cells/frame

## Credits

Game by Debjeet Das.

## License

MIT. See `LICENSE`.
