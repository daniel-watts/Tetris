# Tetris

A complete, self-contained Tetris game served by an `nginx:alpine` container. The entire game (HTML, CSS, JavaScript) lives in a single `index.html` — no build step, no external dependencies.

## Run it

```bash
docker compose up --build -d
```

Then open [http://localhost:8089](http://localhost:8089).

Stop with:

```bash
docker compose down
```

## Controls

| Key            | Action       |
| -------------- | ------------ |
| &larr; / &rarr; | Move         |
| &uarr;         | Rotate CW    |
| &darr;         | Soft drop    |
| Enter          | Hard drop    |
| C / Shift      | Hold piece   |
| Space / P      | Pause toggle |
| R              | Restart      |

Hold can only be used once per piece — the limit resets when the piece locks.

## Features

- 7-bag randomizer (each tetromino appears once per 7-piece cycle)
- Ghost piece (faint preview of where the piece will land)
- Hold slot with the one-swap-per-piece rule
- Next-piece preview
- Score, lines, level
  - Single = 100, Double = 300, Triple = 500, Tetris = 800, each &times; level
  - Soft drop +1 per cell, hard drop +2 per cell
  - Level increments every 10 lines; gravity speeds up each level
- Line-clear animation: flash &rarr; fade out &rarr; blocks above float down to fill the gap
- DAS-lite key repeat for left/right/soft-drop
- Game over with restart prompt

## Project layout

```
.
├── index.html          Game (HTML + inline CSS + inline JS)
├── default.conf        nginx site config (disables HTTP caching)
├── Dockerfile          Two-line nginx:alpine image
├── docker-compose.yml  Builds image, maps host 8089 -> container 80
├── .dockerignore
└── README.md
```

## Notes

- The nginx config sets `Cache-Control: no-cache` so the browser always picks up changes to `index.html`. If you ever see stale behaviour, hard-reload with **Cmd+Shift+R** (or Ctrl+Shift+R).
- To iterate on the game, edit `index.html` and run `docker compose up --build -d` again — the rebuild is sub-second since only the `COPY` layer changes.
