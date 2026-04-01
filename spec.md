# Connect 4 — Game Specification

## Overview

A browser-based Connect 4 game implemented as a single HTML file with embedded CSS and JavaScript. The game supports both local two-player mode and a single-player mode against an AI opponent. Yellow pieces display a smiley face; red pieces display an angry face.

---

## Game Rules

- The board is a 6-row × 7-column grid.
- Two players alternate turns dropping colored pieces into any non-full column.
- A piece falls to the lowest available row in the chosen column.
- The first player to connect four of their pieces in a row — horizontally, vertically, or diagonally — wins.
- If the board fills with no winner, the game ends in a draw.

---

## Visual Design

- **Theme:** Classic — deep blue board, red and yellow pieces on a dark background.
- **Red pieces:** Display an angry face (furrowed brows, frown, glaring eyes) rendered in SVG.
- **Yellow pieces:** Display a smiley face (arched brows, smile, bright eyes) rendered in SVG.
- **Winning highlight:** The four winning cells are highlighted/pulsed to clearly indicate the winning line.
- **Drop animation:** Pieces animate downward into their final row on placement.
- **Hover preview:** A ghost piece appears at the top of each column on hover to indicate where the next piece will land.

---

## Game Modes

### Two-Player Local
- Player 1: Red (angry face), goes first.
- Player 2: Yellow (smiley face), goes second.
- Players take turns clicking columns on the same device.

### Single-Player vs AI
- Human plays as Red (Player 1, goes first).
- AI plays as Yellow.
- AI uses the minimax algorithm with alpha-beta pruning at a search depth of 6 plies.
- A short delay (400ms) is applied before the AI moves to feel natural.

---

## Features

| Feature | Details |
|---|---|
| Game modes | 2-Player Local, vs AI |
| Score tracking | Persistent across rounds until page reload |
| Drop animation | CSS keyframe fall with easing |
| Hover preview | Ghost piece in column header row |
| Win detection | Horizontal, vertical, diagonal (both directions) |
| Win highlight | Winning 4 cells pulse with a glow animation |
| Draw detection | Triggers when board is full with no winner |
| Restart | "Play Again" button resets the board, preserves scores |
| Mode toggle | Switch between 2-Player and vs AI before/between games |

---

## AI Design

- **Algorithm:** Minimax with alpha-beta pruning.
- **Depth:** 6 (configurable via `AI_DEPTH` constant).
- **Heuristic scoring:**
  - Prioritizes center column control.
  - Scores windows of 4 cells based on piece counts: 4-in-a-row, 3-in-a-row + 1 empty, 2-in-a-row + 2 empty, and opponent threats.
- **Move ordering:** Center columns are tried first to improve pruning efficiency.

---

## File Structure

```
index.html   — Single-file game (HTML + CSS + JavaScript)
spec.md      — This document
CLAUDE.md    — Developer guidelines for AI-assisted development
```

---

## Browser Compatibility

Targets modern evergreen browsers (Chrome, Firefox, Safari, Edge). No build step or dependencies required — open `index.html` directly in a browser.
