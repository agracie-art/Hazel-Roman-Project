# Roman Egg Grab

Browser game prototype. Single file: `index.html` (HTML, CSS and JS inline, three.js r128 from cdnjs).
Run it: `python3 -m http.server 8000` then open http://localhost:8000 (landscape, works on phone too).

Read `docs/design.md` for the full game design before changing mechanics.

## Pitch
A "Steal a Brainrot" / "Steal an Egg" style game with a Roman theme, designed by a kid. Run from your base
into the arena, grab an egg, bring it back before the soldier kicks you out. Hatch eggs to get Romans, each
with a real historical fact. Keep them or sell them for money.

## Style rules
- Landscape only. Pixelated 3D: render at ~190px height, canvas scaled up with `image-rendering: pixelated`.
- Low-poly flat shaded boxes, no antialiasing, no textures.
- Roman look: base is a temple, arena is a Colosseum.
- Text is kid-readable and short. Facts are shown exactly as the designer wrote them (see design.md).

## Code map (index.html)
- `RAR`, `ECOL`, `CSS`, `PRICE`, `ROMANS`: data tables (rarity names, colours, sell values, cards with facts)
- `req(t)`: speed needed to carry an egg of tier t (150 + 50*t)
- Scene setup: ground, temple (`BX`), treadmill (`TX`,`TZ`), Colosseum walls (`ACX`,`AR`, gate on the left)
- `person()`: builds the blocky player and soldier models
- `slots`: the 68 arena eggs, tier rises with distance from the gate. `BSLOT`: 6 base egg slots
- `update(dt)`: movement, treadmill, pickup, deposit, soldier AI, kick, camera
- Dialogs: Facts collection and the hatch screen (Keep or Sell)

## Tuning knobs
- Player walk speed 7 times carry multiplier `clamp(speed / req(tier), .2, 1)`
- Soldier (Centurion) patrol 2.5, chase 5.2, kick range 2.4, cooldown 2.5s. A kick flings you in an arc back to the base (1.3s)
- Treadmill gives +6 speed per second (player runs in place, speed pill turns green, HUD shows total gained). Start speed 120
- 68 arena eggs: 16/14/12/10/8/5/3 for tiers 0-6, tier rises with distance from the gate. Base eggs are saved
- Egg respawn 8s (4s after a kick). Sell price is PRICE[tier] * 5

## Known gaps and ideas (not built yet)
- Soldier only chases while you carry an egg. Confirm with designer whether he should also guard back eggs
- No shop or base upgrades yet (original sketch had "Base upgrades", "Selling shop", "Shop up")
- Kept Romans are not shown in the base, only unlocked in Facts. Could display them standing in the temple
- No sound, no save data, no multiplayer
- Pixel-art character faces, better Colosseum arches, hatching animation

## Controls
- Walk: joystick or WASD. Jump: JUMP button or Space. Zoom: +/- buttons, two-finger pinch, mouse wheel, or +/- keys
- Kept Romans walk around the roofless temple (`LOOK` table = their costumes, `addWalker`). Facts list shows a pixel portrait per Roman (`portrait`)
