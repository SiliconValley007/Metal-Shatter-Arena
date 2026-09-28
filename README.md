# Metal Shatter Arena

A single-file, browser-based top-down neon mech arena shooter with destructible voxel enemies and terrain.

## Play

Open `index.html` directly, or via GitHub Pages.

## Controls

- **Move:** WASD / Arrow keys / left stick (touch or gamepad)
- **Aim/Fire:** Mouse (aim + click/hold), right stick, or right gamepad stick
- **Shield:** hold Shift, gamepad bumper, or the on-screen SHIELD button — 90° arc toward aim. Absorbs hits in that cone; HP takes the rest. Regenerates only while not held.
- **Weapons:** 1 Autocannon (deep) · 2 Plasma (wide) · 3 Rocket (radial)
- **Pause:** II button, Esc, or P

Touch, mouse, keyboard, and gamepad share the same combat loop on desktop, tablet, and phone.

## Combat

- Enemy hulls use an offscreen Armor → Hull → Core voxel mask, erased in local space (`destination-out`) so holes stay aligned while units rotate.
- Autocannon punches a deep tunnel, plasma burns a wide disc, rockets erase a radial blast.
- Heat meter, directional shield arc, weapon HUD, camera shake / explosion zoom / velocity lag.

## Tech notes

- Pure HTML/CSS/JS, `<canvas>` 2D, no libraries or network requests.
- Destructible terrain via a per-pixel solidity buffer (`Uint8Array`).
- Enemy navigation: BFS flow-field over a coarse grid.
- Object pools for particles, debris, projectiles, and damage popups.
- Spatial audio with distance gain and stereo pan; `plasmaHumStop()` kills the plasma loop on pause, weapon switch, overheat, death, and restart.
- Player IK: hips from torso rotation `hx = x + cos(θ ± π/2) * d`; planted step targets; lerp to rest when idle.
- Built-in `headlessTest` runs on load (movement, all weapons, camera bounds, mask alignment, audio lifecycle, IK).
