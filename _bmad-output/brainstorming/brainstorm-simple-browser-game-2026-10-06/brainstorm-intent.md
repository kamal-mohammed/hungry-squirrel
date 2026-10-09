# Hungry Squirrel — Brainstorm Intent

Date: 2026-10-06 | Source: brainstorming session (mode: partner, status: complete)

## Concept

Hungry Squirrel is a single-file browser game: a squirrel runs along the ground under a tree, catching falling acorns and dodging falling branches while the pace ramps up. Score is physical — a pile of acorns growing at the base of the tree. The core insight: the game is really about beating your friend's pile; the social rivalry is the core and the catching is the vehicle.

## Goals & Constraints

- Primary goal: learn game development. Secondary: give others something fun to play.
- Shippable in 2-3 days.
- Browser-based; one HTML file (canvas + one JS file), zero backend, zero asset pipeline.
- Must survive the "Bus Stop Playtest": no instructions, no login, one thumb, abandonable mid-play without regret.
- Zero text: every reward and warning is visual or audible, so it works for anyone on any phone.

## Core Loop

- Control: drag left/right with one thumb; squirrel moves horizontally and follows the thumb with a slight lag (feels like a creature chasing the finger, not a cursor).
- Collect: acorns fall; caught acorns add to a visible pile at the base of the tree (the score).
- Avoid: branches fall and knock the squirrel down (on its back ~2s, legs kicking; acorns landing in that window bounce off its belly and are lost).
- Danger telegraphing: only dangerous things cast a shadow on the ground; the shadow grows as the branch nears. Acorns cast none.
- Lives: three hits and you're out. Lives are shown as three acorns in the squirrel's cheeks; each hit pops one out. No hearts, no HUD text.
- Difficulty: speed ramp over time.
- Game over: squirrel climbs down and sits on the pile; pile size IS the end screen.

## Teaching Without Text

- First acorn falls straight at the squirrel — caught with no input; teaches catching = good.
- Second acorn falls one thumb-width to the side — player drags by reflex; two acorns teach the full control scheme.
- Branch shadows teach dodging before the first hit.

## Social Hook

- Share = copy link. Friend's score is encoded in the URL; no server, accounts or login.
- Opening the link shows the friend's pile as a faint grey ghost pile next to the tree at that height (~20 lines of code).
- Passing the ghost pile: squirrel does a little dance; crowd of squirrels cheers from the tree, heads briefly poking out of the leaves. Beating it buries the ghost under your real pile.

## Audio Palette

- Acorn catch: tiny dry crunch; pitch rises with each consecutive catch (streak = climbing scale), a miss resets it.
- Branch hit: loud thud + squirrel squeak; squeak gets more pathetic per life lost (surprised eek, longer whine, sad deflating squeak on the third as the squirrel climbs down).
- Wind: ambient wind in the tree that intensifies as the game speeds up — the wind is the difficulty meter and the in-fiction cause (gusts drop acorns, strong gusts snap branches; shadow appears right after a gust sound so players learn gust = look up).
- Ghost pile beaten: squirrel crowd cheer during the dance.
- Game over: contented munch as the squirrel sits on its pile — losing feels like a satisfied ending, not a punishment.
- All feasible with free SFX or Web Audio synthesis.

## Scope — MoSCoW (agreed, no changes)

**MUST (day 1):** one-thumb drag with follow lag + smooth controls; acorns fall and pile grows; branches with shadow warning knock down; 3 cheek-acorn lives; squirrel sits on pile at game over; ghost pile via share URL + copy-link; placeholder art (emoji / free sprite); speed ramp.

**SHOULD (day 2):** real squirrel art flipped on direction change; textless tutorial (first acorn straight down, second to the side); core sounds (crunch, thud + squeak, contented munch); dance on passing ghost pile.

**COULD (day 3):** rising-pitch streak crunch; escalating squeaks per life; wind ambience rising with speed and gusting before branches; squirrel crowd cheer + heads poking out; knocked-down belly-bounce animation.

**WON'T this time:** rotten acorns, leaves, any server / accounts / leaderboard / daily mode — saved for version two.

Priority order after the MVP: squirrel art first (the character is the charm; a rectangle does not make anyone want to share), then everything else.

## Quality Bar / Definition of Done

- Smooth controls. If the drag does not feel responsive and fluid, the game is not ready to hand to a stranger.
- MVP = core loop (drag, catch, dodge, 3 hits, pile) + ghost pile via URL.
- Art source for a non-artist: free sprite pack or scaled emoji (squirrel, acorn) flipped on direction change; real sprites only if time remains on day three.

## Open Questions

- Difficulty ramp curve (rate of speed increase, acorn/branch frequency) — not discussed.
- Round length / typical session duration — not discussed (Bus Stop framing implies ~90 seconds, but nothing was decided).
- Share-link details not specified: URL encoding format, how the copy-link action is triggered without text.
- Moot for v1 (cut): what leaves and rotten acorns do when caught was left partly undefined; rotten acorns had a faint-shadow and slowdown proposal, leaves were decoys that obscure acorns.
