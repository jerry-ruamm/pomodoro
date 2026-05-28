# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Single-file pomodoro timer app with no build step or dependencies. Open `pomodoro.html` directly in a browser.

## How to run

```bash
start pomodoro.html   # Windows: opens in default browser
```

No build, lint, or test commands — this is a zero-tooling vanilla web app.

## Architecture

`pomodoro.html` — self-contained HTML/CSS/JS, ~570 lines. Three sections:

1. **CSS variables** (`:root`, lines 8-18) — theming via custom properties (dark palette, work/break colors)
2. **HTML structure** — mode tabs, SVG ring timer, control buttons, session counter, duration settings
3. **Script** — `setInterval`-based countdown loop with a state machine

### Timer state machine

Three modes cycled automatically: `work` → `shortBreak` (every 4th → `longBreak`) → `work` → ...

- `startTimer()` / `pauseTimer()` / `resetTimer()` / `skipSession()` are the top-level controls
- `finishSession()` handles completion: increments session count, auto-switches mode, triggers notification, and starts a 3-second `autoStartId` timeout to resume
- Keyboard shortcuts: **Space** toggles, **R** resets, **S** skips

### SVG ring progress

The ring uses `stroke-dasharray` / `stroke-dashoffset` on a circle with circumference `2π × 92 ≈ 578.05`. `updateDisplay()` calculates the offset from `(totalTime - timeLeft) / totalTime`.

Settings inputs (work/shortBreak/longBreak minutes) only apply when the timer is paused (`!isRunning` guard in the change handler).
