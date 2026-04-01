# CLAUDE.md — Developer Guidelines

This file contains instructions for AI-assisted development on this Connect 4 project.

---

## Project Overview

A single-file browser game (`index.html`) implementing Connect 4. All HTML, CSS, and JavaScript live in one file — no build tools, no dependencies, no bundler. Open `index.html` in any modern browser and it works.

---

## Architecture

The JavaScript is organized into distinct logical sections within `index.html`. Keep them in order:

1. **Constants** — Board dimensions, AI depth, timing values.
2. **State** — Mutable game state (board array, current player, scores, mode, etc.).
3. **Board logic** — Pure functions: `dropPiece`, `checkWin`, `isDraw`, `getAvailableRow`, `isValidColumn`.
4. **AI** — `minimax` with alpha-beta pruning, `scoreWindow`, `scoreBoard`, `getBestMove`.
5. **Rendering** — DOM manipulation: `renderBoard`, `renderCell`, `animateDrop`, `highlightWin`.
6. **UI / Event handlers** — Column click, hover preview, mode toggle, restart button, score update.
7. **Init** — `initGame()` called on page load.

---

## Coding Conventions

- **Plain ES6+ JavaScript** — use `const`/`let`, arrow functions, template literals. No TypeScript, no transpilation.
- **No external libraries** — everything is vanilla JS and CSS.
- **Pure functions for logic** — board logic functions must not mutate global state; pass the board as a parameter and return results.
- **Separate concerns** — do not mix rendering code into AI or game logic functions.
- **Named constants** — never use magic numbers. Add new constants to the Constants section at the top.
- **Comments** — add a short JSDoc-style comment above any non-obvious function.

---

## Key Constants

```js
const ROWS = 6;
const COLS = 7;
const AI_DEPTH = 6;        // Minimax search depth — increase for stronger AI (slower)
const AI_DELAY_MS = 400;   // Pause before AI moves (UX feel)
const PLAYER = 1;          // Human / Player 1 (Red)
const AI = 2;              // AI / Player 2 (Yellow)
const EMPTY = 0;
```

---

## State Shape

```js
const state = {
  board: [],           // 2D array [row][col], values: EMPTY | PLAYER | AI
  currentPlayer: 1,    // 1 = Red, 2 = Yellow
  gameOver: false,
  mode: 'ai',          // 'ai' | '2p'
  scores: { 1: 0, 2: 0 },
  animating: false,    // lock input during drop animation
};
```

---

## Board Representation

- `board[0]` is the **top** row; `board[ROWS-1]` is the **bottom** row.
- Pieces drop to the highest available index in a column (lowest visual row).
- A cell value of `0` = empty, `1` = Player 1 (Red), `2` = Player 2 (Yellow).

---

## Adding Features

### New game mode
1. Add a mode string to the `mode` toggle logic in the UI section.
2. Gate AI move triggering on `state.mode === 'ai'`.

### Stronger AI
- Increase `AI_DEPTH` (each +1 roughly doubles computation time).
- Improve `scoreWindow()` to weight threats more precisely.
- Add transposition table caching to `minimax()`.

### New visual themes
- CSS custom properties (variables) are declared on `:root`. Add new theme variables there and toggle a class on `<body>` to switch themes.

### Sound effects
- Hook into the drop animation completion callback to play a short audio clip via the Web Audio API.

---

## Testing

There is no automated test suite. To manually verify:

- Drop a piece in every column and confirm it lands in the correct row.
- Fill a column and confirm it becomes unclickable.
- Test all four win directions: horizontal, vertical, diagonal ↗, diagonal ↘.
- Fill the board without a winner and confirm draw is detected.
- In AI mode, verify the AI takes a winning move when available and blocks the human's winning move.

---

## What NOT to Do

- Do not split this into multiple files without updating this guide and the spec.
- Do not add a framework (React, Vue, etc.) without significant justification.
- Do not use `innerHTML` with unsanitized user input.
- Do not let AI logic run synchronously on the main thread for depth > 7 — it will freeze the UI. Use a Web Worker for deeper searches.
- Do not mutate the board array directly inside `minimax` — always clone it (use `board.map(r => [...r])`).
