# Steal a Roman

Browser game prototype. Game lives in `public/` (served by Cloudflare). Single file: `public/index.html` (HTML, CSS and JS inline, three.js r128 from cdnjs).
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
- `req(t)`: speed needed to carry an egg of tier t (`REQ` = 150, 220, 350, 600, 1000, 1600, 2500)
- Scene setup: ground, temple (`BX`), treadmill (`TX`,`TZ`), Colosseum walls (`ACX`,`AR`, gate on the left)
- `person()`: builds the blocky player and soldier models
- `slots`: the 50 arena eggs, tier rises with distance from the gate. `BSLOT`: 6 base egg slots
- `update(dt)`: movement, treadmill, pickup, deposit, soldier AI, kick, camera
- Dialogs: Facts collection and the hatch screen (Keep or Sell)

## Tuning knobs
- Player walk speed 7 times carry multiplier `clamp(speed / req(tier), .2, 1)`
- Soldier (Centurion) patrol 2.5, chase 5.2, kick range 2.4, cooldown 2.5s. A kick flings you in an arc back to the base (1.3s)
- Treadmill gives +6 speed per second (player runs in place, speed pill turns green, HUD shows total gained). Start speed 120
- 50 arena eggs: 11/11/7/6/5/5/5 for tiers 0-6, tier rises with distance from the gate. Base eggs are saved
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
- Watch-an-ad button (top right): plays public/ad.mp4 (not cached by the service worker), pays AD_REWARD after AD_MIN seconds watched, 60s cooldown. Close with X, Skip or Escape
- Shop (button, top right): `openShop()`. Stronger base (`CAPS` pads 6/9/12/15/18, `BASE_COST`, `applyBase()`), buy speed (+`SPEED_STEP`, `speedCost()` = 50*(buys+1)), VIP (`VIP_COST`). Saved in `st.baseLv`, `st.speedBuys`, `st.vip`
- VIP: gold crown + VIP label over the player (`applyVip()`), VIP Lounge south of the base (`VX0..VZ1`) is roped off without VIP (solid, flashes `#vipPrompt`), has a x10 speed treadmill (VIP_RATE 60/s vs NORMAL_RATE 6/s)
- Hatch fanfare scales by rarity (`FX` table: crack time, cracks, shell pieces, rings, light beams, screen shake, banner). Secret is rainbow
- Speed resets to BASE_SPEED (120) plus 20 per speed upgrade bought in the Shop every time an egg is delivered to the base (not on hatch)
- Arena eggs are placed in random clumps each load (10 clump centres, min spacing 3.4, clear lane inside the gate). Rarity only loosely follows distance from the gate, so rare eggs can be anywhere. Pick-up radius 1.15
- Upgrade panel (#upg, `upgSync`): shows Upgrade base + Get VIP when you are in the base, only Get VIP at the VIP rope. Not enough cash opens `needCash()` with a Watch Ad +$50 button; after a paid ad the pending purchase (`pendingBuy`) runs automatically. The Shop dialog also has a Watch Ad row
- Reset progress: red button at the bottom of the Shop dialog, asks to confirm, then clears localStorage key reg-save and reloads (`confirmReset()`)
- HUD layout: Facts + Speed + Eggs top-left; cash, Shop and Watch Ad (W2E) top-right; zoom +/- low on the right edge above JUMP. No toast-style direction hints
- Upgrade popup (#upg) opens when you ENTER the base (or touch the VIP rope); closes with X and stays closed until you leave and re-enter. You spawn inside the base so it does not open at the start
- In-world guidance: bouncing arrows along the path (red toward the Colosseum, green back toward the base, `updateGuide`), a big bobbing arrow over the gate / hatch pads, a turning arrow over your head, and a bouncing arrow over every egg waiting to be hatched
- Safe zone: everything outside the gate (x < ACX-AR). Green floor + SAFE ZONE signs outside, red DANGER sign inside. The Centurion only chases inside the walls
- Kept Romans earn cash per second (`INCOME` per tier = .1,.3,.8,2,5,12,30 /s; ~sell price per minute). First 14 kept walk the temple and pay out as floating +$ pops every ~2s; extra kept Romans earn silently. HUD shows total +$/s under the balance
- Audio is generated in code (WebAudio, no files): `AU` engine, `auStep` patterns, `sfx(name)`. Calm jovial lyre/hand-drum tune when not carrying an egg; tense Phrygian war music (low brass + taiko) when carrying (louder when the Centurion is within 20); crossfade via `AU.tgt`/`auMood`. SFX: pickup, fling, deliver, coin, buy, crack, burst (scales by rarity), unlock, jump. Starts on Play; ♪ button mutes (saved in localStorage reg-mute); music ducks during ads/dialogs
- Rarer eggs incubate before they hatch (`HATCH_WAIT` seconds per tier: 0,5,15,30,60,120,240). Tap an egg to start the timer; an in-world tag above the egg shows the countdown + progress bar. When done it turns READY (chime, bouncing arrow, and a green 'Egg ready! Tap to hatch' button at the top) and waits for you to tap, so you are always watching the show; the camera swings to the egg when the hatch starts. Timers use real time (`e.end`) so they keep running while the game is closed
- Hatch animation is switched OFF for now (`HATCH_ANIM=false` in index.html): hatching goes straight to the Roman + fact popup. The wobble/crack/burst/Roman-reveal code (`FX`, `updateHatch`, `burst`) is kept; set the flag to true to bring it back
