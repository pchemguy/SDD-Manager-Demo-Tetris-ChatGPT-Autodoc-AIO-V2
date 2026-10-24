# Component decomposition

## Domain components

[ARCHITECTURE](ARCHITECTURE.md) owns the block arrangement. The components below are logical boundaries, not a requirement for a file per function. Shared value types describe cells, pieces, board, actions, settings and snapshots; they contain no browser handles.

| Component | Owns / provides | Requires / excludes | Verification seam |
| --- | --- | --- | --- |
| Geometry and board | Tetromino cell transforms, bounds/overlap checks, atomic placement and row compaction | Piece/board values; no timer, scoring or rendering | Small board fixtures and rotation/bounds properties |
| Landing | First-obstruction regular landing; lowest legal penetrator landing; grounded predicates | Geometry and settled board; no mutations | Same fixture result consumed by ghost/drop/locking |
| Piece source | Uniform baseline source and 7-bag sequence, upcoming-piece consumption | Supplied random sample function; no UI | Seeded sequences, bag membership and queue consumption |
| Game transitions | Session lifecycle; move/rotate/drop/Hold/activate; lock-clear-spawn; score/level/bonuses | Geometry, landing, source and lock clock; no persistence | Action traces with explicit elapsed times and deterministic source |
| Lock clock | First-grounding deadline, policy capture, expiry and pause semantics | Running simulation time and grounded observations; no wall clock | Boundary times, no-reset moves, airborne expiry |
| Projection | Presentation snapshot, ghost visibility and bonus symbols derived from state | Landing and validated settings; no board writes | Midline fixtures and white/regular color separation |

Game transitions own all authoritative session mutation and coordinate placement → clear → scoring/bonus → spawn as one operation. Landing supplies a position or explicit absence; it never silently converts a penetrator to a regular piece. Source consumption is explicit so first Hold, swap Hold and rejected actions cannot accidentally consume different queue lengths.

## Controller and browser components

| Component | Owns / provides | Required boundary | Verification seam |
| --- | --- | --- | --- |
| Application controller | Current domain state, configuration state, captured piece settings, start/restart/pause and snapshot dispatch | Domain commands and adapter interfaces | Fake clock/input/storage ports with real domain |
| Input adapter | Semantic key actions, repeat scheduling and focus guards | Controller action sink | Browser focus and keyboard tests, no actions from form controls |
| Animation clock | Monotonic frame deltas and suspension/resume origin | Controller time sink | Fake monotonic clock plus hidden-tab integration |
| Board renderer | Canvas grid, settled/active cells and ghost | Read-only snapshot and sizing context | Browser screenshots and geometry/color assertions |
| Panel renderer | Score, level, next/Hold previews, bonus indicator, settings and persistence status | Read-only snapshot; validated setting-change sink | Native control labels, disabled states and visible status |
| Settings schema/resolver | Typed six-setting schema; validation and independent precedence | URL/cookie strings | Table-driven malformed, absent and mixed-source fixtures |
| Persistence adapter | Read cookies/query, write selected values, replace URL without reload | Validated schema values; Cookie/History APIs | Real reload/history/cookie tests and blocked-port checks |

The input adapter emits semantic actions rather than editing state. The controller drops actions while paused/over and applies availability settings before calling transitions. Renderers may cache graphics but must not own gameplay truth. Settings events cannot be mistaken for gameplay keys.

## Interface and lifecycle rules

The game boundary accepts create-session, semantic action and advance-time operations with explicit settings, random source and elapsed time as appropriate. It returns a state/snapshot without relying on DOM execution. Final TypeScript signatures may follow local implementation needs; SPEC owns observable contracts and error outcomes.

Landing receives board, piece pose and transit mode and returns the terminal pose or no legal position. Both projection and hard drop query this boundary. A move or rotation is proposed, validated and committed atomically; rejected actions preserve the whole prior state, including bonus balance and deadlines.

The controller creates one input subscription and one animation loop per mounted app; restart resets domain state without duplicating listeners. Teardown cancels its own loop/listeners. The clock reports running simulation time only. Domain failure yields a terminal snapshot; a renderer or persistence failure must not write partial board state.

Configuration resolver and persistence adapter are separate because blocked cookie/history access does not invalidate valid in-memory preferences. The adapter returns availability outcomes to the controller; the panel surfaces a concise status. No telemetry or external storage is introduced.

## Dependency and review assessment

Domain relationships are directed: transitions consume geometry, landing, source and clock helpers; projection consumes state and landing; neither feeds browser state back into domain helpers. Controller consumes these values; adapters consume or emit through the controller. There is no dependency cycle or second owner of settled cells, settings precedence or landing.

Each component can be checked with a small input/output or browser interaction seam. Splitting into services, repositories or general plugin registries would add indirection without independent responsibilities. SPEC settles precise coordinates, key mappings, lock-event ordering, failure semantics and persistence encodings; layout owns physical allocation. No component children are necessary at this scale.
