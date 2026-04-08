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

**After every change, commit and push to GitHub.** This is mandatory — never leave work uncommitted. Each saved state on GitHub lets us revert cleanly if something breaks.

1. Stage changed files by name (never `git add .`)
2. Write a clear, specific commit message describing *what changed and why*
3. Push immediately

```
git add shooter.html
git commit -m "feat: add shield power-up that absorbs one hit"
git push
```

Remote: `https://github.com/abhayemadhok/ball-shooter`

### Commit message conventions
- `feat:` — new feature or gameplay addition
- `fix:` — bug fix
- `tweak:` — tuning constants (speeds, intervals, scoring)
- `refactor:` — code restructure with no behaviour change
- `style:` — visual/colour changes only

Keep the subject line under 72 characters. If more context is needed, add a blank line followed by a short body.
