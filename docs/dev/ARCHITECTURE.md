# Architecture

## Context and arrangement

The [project brief](PROJECT.md) defines a static desktop browser game. TypeScript implements the application; Canvas draws the board, while HTML presents controls and readable status. No service, account or network request is required during play. Browser cookies and the current page URL store only configuration.

The intended system has three blocks:

| Block | Responsibility | Dependencies |
| --- | --- | --- |
| Game domain | Board, piece geometry, collision/landing, randomizer, game transitions, scoring and lock timing | TypeScript values and explicitly supplied random samples/time deltas |
| Application controller | Translate player actions and elapsed time into domain commands; own active session, focus/pause, configuration effects and presentation snapshots | Domain and narrow browser adapters |
| Browser adapters | Canvas/HTML presentation, keyboard/focus events, animation clock, cookie/URL access | Browser APIs; consume snapshots and emit actions |

The controller owns the session. Domain functions cannot read the DOM, global clock, cookies, URL or browser random generator. Presentation never mutates board state. Configuration parsing is pure; persistence adapters provide strings and receive validated settings.

## Interaction and state ownership

A browser event becomes a semantic action. The controller supplies that action, a time delta and random input to the domain. Domain transitions produce a complete snapshot; renderers draw it and update controls/status. Animation frames advance simulation only while running. Pause, hidden tabs and loss of focus suspend timing and clear held input; a fresh clock origin prevents resume catch-up.

The settled board contains only locked cells. The active piece, Hold slot, upcoming queue, bonus balance, score, level, lock deadline and gameplay status are separate session fields. Board mutation occurs only in an atomic legal lock/clear/spawn transition. This prevents intermediate penetrator overlaps from leaking into the settled board or being rendered as destroyed cells.

One geometry/landing service owns regular and penetrator landing queries. Hard drop, ghost projection and terminal penetrator grounding consume its result. Regular landing stops at the first obstruction; penetrator landing searches through occupied rows for the lowest legal terminal position. A legal lock requires in-bounds, non-overlapping cells even when transit collision is ignored.

The current piece captures locking policy on spawn and the gravity interval on first grounding. Settings own future preferences separately from this captured state. Hold creates a new active piece lifecycle while preserving the once-per-lock restriction. Restart creates a fresh game session and keeps the selected configuration.

## Configuration boundary

A typed settings schema owns allowed values, defaults and stable external keys. A pure resolver applies URL → cookie → default independently per setting. Browser persistence writes only validated values, preserving unrelated URL parameters and omitted cookies. Storage and history failures cannot corrupt the game state: in-memory configuration remains usable and the UI reports unavailable persistence.

## Rationale and invariants

Canvas keeps board rendering small and predictable; native HTML gives settings labels, focus and standard control behavior. A framework and state-management library are unnecessary. A reducer-like transition boundary with immutable input/output semantics gives deterministic verification without requiring a general event bus, plugin framework or entity system.

The dependency direction remains adapters/controller → domain. Components share typed values rather than mutable global stores. Domain and configuration logic can be verified without a browser; integration checks use actual keyboard events, Canvas output, cookies and URL state in isolated Chromium and Firefox environments.

System-wide invariants are legal atomic locks, a read-only settled stack during transit, a single landing definition for preview/drop, bounded bonuses, no white regular pieces and per-setting persistence precedence. [DECOMPOSITION](DECOMPOSITION.md) owns logical interfaces; SPEC owns precise behavior and acceptance. No focused architectural children are required for this scale.
