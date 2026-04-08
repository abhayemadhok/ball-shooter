# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

A single-file 2D browser shooter game. No build system, no dependencies, no package manager — just `shooter.html`.

## Running the Game

Open `shooter.html` directly in any browser. On Windows:

```
start shooter.html
```

## Architecture

Everything lives in `shooter.html` — inline `<style>`, a `<canvas>` element, and a single `<script>` block organized as follows:

- **Constants** — all tunable values (speeds, radii, intervals, colors) at the top of the script
- **`state` object** — single source of truth; `resetState()` reinitializes it completely for restarts
- **Input module** — `keydown/keyup`, `mousemove`, `mousedown/mouseup` listeners write into `state.input`
- **Update functions** — `updateDifficulty`, `spawnEnemies`, `updatePlayer`, `updateBullets`, `updateEnemies`, `checkCollisions` — all read from `state`, never from DOM events directly
- **Draw functions** — `drawBackground`, `drawPlayer`, `drawBullets`, `drawEnemies`, `drawHUD`, `drawGameOver`
- **`gameLoop`** — `requestAnimationFrame` loop, always running; dispatches on `state.phase` (`"playing"` | `"gameover"`)

### Key design rules
- Delta time is in **seconds** (not ms), capped at `0.1s` to prevent teleporting on the first frame
- Collision detection uses the **`active` flag** pattern — never mutate arrays mid-loop; filter once per frame after all checks
- Enemy velocity is fixed at **spawn time** (aimed at player position then), not recalculated each frame
- `resetState()` is the only restart mechanism — no page reload

## Git Workflow

Commit and push after every meaningful change:

```
git add shooter.html
git commit -m "short description"
git push
```

Remote: `https://github.com/abhayemadhok/ball-shooter`
