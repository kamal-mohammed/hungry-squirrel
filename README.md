# Hungry Squirrel

A one-thumb browser game: a squirrel runs under an oak tree catching falling acorns and dodging falling branches. Your score is the pile of acorns at the foot of the tree.

**Play it:** https://kamal-mohammed.github.io/hungry-squirrel/

## How to play

- Drag left and right with a finger, or use the arrow keys (A and D also work).
- Catch acorns to grow your pile. Branches cast a shadow before they land; dodge them. Three hits and the squirrel climbs down to sit on its pile.
- Missed acorns stay on the ground and slowly raise it. If the ground reaches the canopy, the game ends.
- Space starts, pauses and resumes. P or Escape also pause.

## Special acorns

| Acorn | Effect |
| --- | --- |
| Golden | Restores one life (up to three) |
| Rotten (greenish, faint shadow) | Slows the squirrel for a few seconds |
| Silver sweeper (blue glow, spinning ring) | Clears half the acorns on the ground and lowers it |
| Ruby x10 | Adds ten acorns to the pile |

## High score

Your best pile is saved in the browser and shown on the start screen, in the top bar while you play, and on the game over screen. Beat it and the trophy flashes gold.

## Sharing

The share button copies a link with your score. Opening it shows your pile as a grey ghost next to the tree, so a friend can try to beat it. Pass it and the squirrel does a little dance.

## Tech

One HTML file. The scene is rendered with [Three.js](https://threejs.org/) (bundled as `three.min.js`, with a CDN fallback), the sound and music are synthesised with Web Audio, and there is no backend or build step. `index-2d.html` is the earlier canvas-only version.

To run locally, open `index.html` in a browser, or serve the folder with any static file server.
