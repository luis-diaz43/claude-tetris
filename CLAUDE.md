# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the game

No build step or dependencies. Open directly in a browser:

```bash
start index.html        # Windows
# or serve with any static server:
python3 -m http.server 8000
npx serve .
```

There are no tests, no linter config, and no package.json.

## Architecture

All game logic lives in `game.js` (~305 lines). `index.html` provides the DOM shell; `style.css` handles dark/retro styling. No modules, no classes — the file uses module-level `let` variables for mutable state.

**Key global state** (all reset by `init()`):
- `board` — `ROWS × COLS` matrix; cells hold `0` (empty) or a color index `1–7`
- `current` / `next` — piece objects `{ type, shape, x, y }`
- `dropInterval` — fall speed in ms, recalculated on level-up

**Game loop**: `requestAnimationFrame`-based `loop(ts)` accumulates `dropAccum`; fires gravity when `dropAccum >= dropInterval`. `animId` holds the pending frame handle so `cancelAnimationFrame` can stop it cleanly.

**Piece rotation**: `rotateCW(shape)` does transpose + row-reverse. `tryRotate()` tries 5 kick offsets `[0, -1, 1, -2, 2]` before giving up.

**Rendering**: Two canvases — `#board` (300×600) for the playfield, `#next-canvas` (120×120) for the preview. Ghost piece is drawn at `globalAlpha = 0.2` before the active piece.

## Tunable constants (top of `game.js`)

| Constant | Default | Note |
|---|---|---|
| `COLS` / `ROWS` | 10 / 20 | If changed, update `<canvas>` `width`/`height` in `index.html` to `COLS×BLOCK` / `ROWS×BLOCK` |
| `BLOCK` | 30 | Pixel size of each cell |
| `LINE_SCORES` | `[0,100,300,500,800]` | Points for 1–4 cleared lines, multiplied by current level |
