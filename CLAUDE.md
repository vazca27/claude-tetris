# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Vanilla JS Tetris (HTML5 Canvas). No dependencies, no `package.json`, no build step, no tests, no linter. The README and UI strings are in Spanish.

## Running

Open `index.html` directly, or serve statically (e.g. `python -m http.server 8000`, then visit `http://localhost:8000`). Verification is manual in the browser.

## Architecture

Three files: `index.html` (DOM + canvases), `style.css`, and `game.js` (all logic, one script, global mutable state).

- `game.js` looks up DOM elements by id at load time (`board`, `next-canvas`, `score`, `lines`, `level`, `overlay`, `overlay-title`, `overlay-score`, `restart-btn`). Renaming ids in `index.html` breaks the script.
- Board is a `ROWS × COLS` matrix of `0` or a piece-type index 1–7. That same index is used in `PIECES` (shape matrices) and `COLORS`, so the three tables must stay aligned.
- State lives in module-level `let` variables, reset by `init()` (also the restart button handler). `current`/`next` are `{type, shape, x, y}` objects; rotation replaces `shape` with a new matrix (`rotateCW`) and `tryRotate` applies simple wall kicks (`0, ±1, ±2`).
- Piece lifecycle: `lockPiece()` → `merge()` → `clearLines()` (updates score/level/`dropInterval`) → `spawn()`, which calls `endGame()` if the new piece collides immediately.
- Game loop is `requestAnimationFrame` (`loop`), accumulating time into `dropAccum` against `dropInterval`. Pause/game over work by cancelling `animId`; unpausing resets `lastTime` and calls `loop` directly. Input is handled in a single `keydown` listener.
- Changing `COLS`/`ROWS`/`BLOCK` requires also updating the `width`/`height` attributes of `<canvas id="board">` in `index.html` (`COLS*BLOCK` × `ROWS*BLOCK`).
