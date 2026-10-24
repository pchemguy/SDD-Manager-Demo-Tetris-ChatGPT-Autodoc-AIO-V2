# System specification

## Scope and authority

The system is the static desktop keyboard game described in [PROJECT](PROJECT.md), structured by [ARCHITECTURE](ARCHITECTURE.md) and [DECOMPOSITION](DECOMPOSITION.md). This specification owns required observable behavior. The numbered contract groups below and their focused children describe the complete intended product; PLAN selects release increments without claiming later capabilities in earlier releases.

| Contract | Canonical detail | Responsible components |
| --- | --- | --- |
| G1 — Board, pieces and lifecycle | [Gameplay](spec/GAMEPLAY.md#board-pieces-and-lifecycle) | Geometry/board, transitions, clock, controller |
| G2 — Movement, rotation and source | [Gameplay](spec/GAMEPLAY.md#movement-rotation-and-piece-source) | Geometry, landing, piece source, input |
| G3 — Gravity and locking | [Gameplay](spec/GAMEPLAY.md#gravity-and-locking) | Lock clock, transitions, controller |
| G4 — Score, Hold and ghost | [Gameplay](spec/GAMEPLAY.md#score-hold-and-ghost) | Transitions, projection, panel |
| G5 — Penetrator | [Gameplay](spec/GAMEPLAY.md#penetrator) | Landing, transitions, projection, panel |
| C1 — Six-setting schema and live behavior | [Configuration](spec/CONFIGURATION.md#schema-and-live-behavior) | Schema/resolver, controller, panel |
| C2 — Resolution, writes and failures | [Configuration](spec/CONFIGURATION.md#persistence-resolution-and-failures) | Resolver, persistence adapter, panel |
| Q1 — Browser interaction and delivery | This document | Controller, browser adapters, packaging |

## Q1 — Browser interaction and delivery

Deliver static assets with no runtime backend, external font, telemetry or CDN dependency. Keyboard play and configuration controls work in Chromium and Firefox using isolated environments under [browser policy](../../BROWSER_DEV_POLICY.md). Browser execution is verified with the actual built application; provisioning success alone is insufficient. Headless interaction and screenshots are sufficient for required coverage. Developer tooling supports Windows CMD and Linux shells under [shell policy](../../WINDOWS_SHELL_POLICY.md).

The page shows board, score, level, lines, game status, controls/help, settings, next/Hold previews when enabled and one lightning symbol per bonus. Regular colors exclude white; active penetrators use white for every occupied cell. Ghosts are visibly distinct outlines or translucent projections. Labels and status remain readable independently of piece color.

Provide labeled native HTML controls, keyboard focus indicators and text status for game over/pause and unavailable persistence. Gameplay handlers ignore keystrokes from input, select, textarea, button and editable elements. Prevent default page scrolling only for handled gameplay keys while the game has focus. Escape from the game focus region remains possible using normal Tab navigation. A restart control is available without reloading the page.

Use one animation loop and one set of subscriptions per mounted game. Repeated restart does not multiply actions. Losing focus or hiding the page pauses a running game and clears held keys. Resumption requires an explicit action; elapsed suspended wall time does not advance gravity or locking. Teardown cancels owned work. Rendering consumes snapshots; DOM controls and errors never directly change settled cells.

## End-to-end acceptance

| Acceptance | Required observable evidence |
| --- | --- |
| A1 — Playable baseline | Start, move, rotate, descend, lock, clear, score, increase level, pause/resume, top out and restart in the built game. Verify no overlap/out-of-board lock. |
| A2 — Modern play | Distinct 7-bag membership; ordered kick behavior; Hold empty/swap/repeat cases; ghost/drop equality; delayed and immediate hard drop. |
| A3 — Timing | At a captured 1000 ms interval, first grounding at t=200 cannot lock before t=1200. Grounded movement and repeated drop do not extend it. Airborne expiry waits for grounding; pause time is excluded. |
| A4 — Penetration | Spend exactly one available bonus; pass through occupied cells without mutation; ghost and drop agree on deepest legal landing; Hold remains disabled; all final cells are legal; white appearance and indicator update. |
| A5 — Configuration | All six controls affect their specified behavior; captured lock policy stays stable for the current piece; disabled preview/Hold/drop have correct UI and action behavior. |
| A6 — Persistence | Independently mix valid URL, cookie and default sources; save explicit defaults; preserve omitted cookies and unrelated URL parameters; reload restores values; invalid values and unavailable storage degrade as specified. |
| A7 — Integrated quality | Both target browsers demonstrate keyboard/form-focus separation, Canvas output, pause/resume, restart, settings/reload and preserved earlier capabilities. Required failures are repaired before phase completion. |

Pure domain acceptance uses explicit time and random inputs; browser acceptance uses actual keys, settings, URL and cookies. Screenshots or pixel/shape checks establish rendering alongside state assertions. No gameplay or browser acceptance is claimed solely from document review.

Non-goals are multiplayer, accounts, backend leaderboards, mobile touch controls and saving an in-progress game. Only configuration persists. Browser internal errors may be surfaced as a concise status; credentials and personal data are never stored by the game.
