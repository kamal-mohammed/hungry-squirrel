# Hungry Squirrel - Three-Day Build Plan

A squirrel drags left and right under a tree, catching falling acorns into a growing pile while dodging branches; a friend's pile travels in the share link as a ghost to beat.

**Quality bar (definition of done):** smooth controls. If the drag does not feel responsive and fluid, the game is not ready to hand to a stranger.

**Constraints:** one self-contained HTML file (canvas + vanilla JS), no backend, no build step, emoji or free sprites for art, Web Audio or free SFX for sound. Zero text in play - every reward and warning is visual.

---

## Day 1 - MUST: a stranger can play it

### Foundation
- [x] Create `index.html` with a full-screen `<canvas>`, a `<script>` block, and a `<meta name="viewport">` tag; disable page scroll/zoom on touch (`touch-action: none`)
- [x] Size the canvas to the window (handle `resize` and devicePixelRatio) so it looks sharp on phones
- [x] Set up the game loop with `requestAnimationFrame`; compute `dt` each frame (or use a fixed timestep accumulator) so speed does not depend on frame rate
- [x] Draw the static scene: ground line, tree trunk at one side, sky; keep everything in a single `state` object
- [x] Define a tiny state machine: `playing` -> `gameover`

### One-thumb drag with follow lag
- [x] Track a `targetX` from pointer events (`pointerdown`/`pointermove`/`pointerup`) so touch and mouse both work
- [x] Add arrow keys (and A/D) for desktop: move `targetX` left/right while held
- [x] Each frame move the squirrel toward `targetX` with a lag (e.g. ease a fraction of the remaining distance per frame) so it chases the finger like a creature, not a cursor
- [x] Clamp the squirrel to the screen edges
- [x] Flip a `facing` flag whenever the squirrel's x velocity changes sign (used for art later)

### Falling acorns, catching, the pile
- [x] Spawn acorns at random x above the screen on a timer; each has x, y, speed
- [x] Move acorns down each frame; remove any that pass the ground (a miss - no penalty in this version)
- [x] Catch detection: acorn overlaps the squirrel's catch box -> remove acorn, `pile += 1`
- [x] Draw the pile at the base of the tree as a mound whose height/width grows with `pile` (the pile IS the score - no HUD text)

### Branches, shadows, lives
- [x] Spawn branches less often than acorns; each branch picks its landing x first
- [x] Before the branch is visible, draw a shadow on the ground at its landing x that grows as the branch gets nearer (only dangerous things cast shadows)
- [x] Branch hits the squirrel -> `lives -= 1`, enter `knockedDown` for ~2s (squirrel on its back, no catching; acorns in that window are lost), then recover
- [x] Draw lives as three acorns in the squirrel's cheeks; each hit pops one out (no hearts, no text)
- [x] Give the player a short invulnerability right after getting up so two branches cannot chain-stun

### Game over and speed ramp
- [x] On the third hit, switch to `gameover`: squirrel climbs down and sits on the pile; the pile size is the end screen
- [x] Tap/click on the game-over screen restarts a fresh run
- [x] Speed ramp: increase acorn fall speed and spawn rate (and branch frequency) gradually with elapsed time

### Ghost pile and share link
- [x] On load, read `?pile=N` from `location.search`; if present, draw a faint grey ghost pile next to the tree at the height N acorns would reach
- [x] When your real pile passes the ghost height, draw your pile over it (the ghost gets buried)
- [x] Add a "share" button (icon only, shown on the game-over screen) that builds `location.origin + pathname + ?pile=<score>` and copies it with `navigator.clipboard.writeText` (fall back to showing the URL selectable if clipboard is denied)
- [x] Reject garbage in the param (`parseInt`, clamp to a sane max)

### Placeholder art and phone test
- [x] Draw the squirrel, acorns, and branches as scaled emoji via `ctx.fillText` (fallback: rectangles and circles if a glyph is missing)
- [ ] Open the file on a real phone (serve the folder locally over your LAN or push it to any static host) and play a full run with one thumb
- [ ] Fix the first three things that feel wrong - drag lag, catch box size, branch fairness - before adding anything else
- [ ] **End of day check:** hand the phone to someone who has never seen it; they should play without a single word of explanation

---

## Day 2 - SHOULD: the character and the feel

### Real squirrel art
- [x] Pick a free sprite pack or a larger emoji squirrel; load it as an `Image` (inline as a data URI to keep the single-file rule) — *done differently: the squirrel is drawn with canvas paths so it looks the same on every device and supports poses*
- [x] Draw it with `ctx.scale(-1, 1)` when `facing` is left so it flips on direction change
- [x] Add a knocked-down pose (on its back) and a sitting pose for game over

### Textless tutorial
- [x] First acorn of each run falls straight down onto the squirrel's starting x - the player catches it without doing anything
- [x] Second acorn falls one thumb-width to the side so the player drags by reflex
- [x] Only after those two does normal random spawning begin

### Core sounds
- [x] Create an `AudioContext` and **unlock it on the first touch/click** (`ctx.resume()` inside the first pointer handler) - browsers block audio until a gesture
- [x] Catch: a tiny dry crunch (short noise burst or free SFX)
- [x] Branch hit: loud thud followed by a squirrel squeak
- [x] Game over: a contented munch as the squirrel sits on the pile - losing should feel like a satisfied ending
- [x] Keep a tiny `play(name)` helper so Day 3 sounds slot in without rewiring

### Ghost dance
- [x] When the real pile first passes the ghost height, trigger a short dance animation on the squirrel (hop/wiggle for a second) without pausing play

---

## Day 3 - COULD: only if Days 1-2 are done

- [x] Streak crunch: each consecutive catch raises the crunch pitch (climbing scale via `playbackRate` or oscillator frequency); a miss resets it
- [x] Escalating squeaks per life lost: surprised eek on the first, longer whine on the second, sad deflating squeak on the third as the squirrel climbs down
- [x] Wind ambience: looping filtered noise whose volume/intensity rises with the speed ramp - the wind is the difficulty meter
- [x] Gust sound just before each branch's shadow appears so players learn gust = look up
- [x] On beating the ghost: a crowd of squirrels cheers from the tree, and a few heads briefly poke out of the leaves during the dance
- [x] Knocked-down animation: legs kicking, and acorns that land during the ~2s bounce off the belly instead of vanishing

### Polish and ship
- [ ] Tune controls one more time on a phone: lag, catch box, max speed
- [x] Test the share link round trip: finish a run, copy, open the link on a second device, confirm the ghost pile shows at the right height and gets buried when beaten
- [x] Check landscape and small screens; make sure nothing is off-canvas
- [ ] Put the file on a static host and send the link to friends

---

## Won't this time (parked for version two)
- Rotten acorns (stomach ache / slowdown, faint shadow)
- Leaves (drifting decoys that obscure acorns)
- Any server, accounts, leaderboard, or daily mode

---

## Tuning notes
Numbers to set by feel on a real phone, not by guessing:
- **Follow lag** - the ease fraction per frame; too low feels sluggish, too high feels like a cursor
- **Catch box size** - how forgiving the acorn/squirrel overlap is
- **Fall speed ramp** - starting speed, how fast it climbs, and the ceiling
- **Spawn rates** - acorns per second at start and at full speed; branch frequency
- **Knockdown duration** - about 2 seconds, plus the short invulnerability after
- **Shadow lead time** - how long the ground shadow shows before the branch arrives (long enough to dodge after one surprise)
- **Ghost pile scale** - pixels per acorn so a typical score is visible but not screen-filling
