# Implementation tasks

The complete intended hierarchy derives from [PLAN](PLAN.md), [SPEC](SPEC.md) and [layout](layout.md). Every task includes its own meaningful tests and affected documentation. Initial implementation status is unchecked.

Execution selects an explicitly authorized range. Required browser or contract failures block affected acceptance. Each completion commits its result/evidence and initial checkbox together, pushes/readbacks, then closes its issue. Reports contain Findings/Blockers and TODO sections; only eligible non-critical code findings may be deferred. Phase integration and release publication follow PLAN after verified phase review; task completion alone is not a published release.

## Phase 1 — Classic baseline

- [ ] Phase 1 — Classic baseline
    - [ ] Milestone 1.1 — Falling-piece play
        - [ ] T-001 — Establish pinned tooling and isolated browser readiness
            Scope: package/lockfile, TypeScript/build/unit/Playwright configs, docs/DEVELOPMENT.md and generated-path exclusions. Contracts: G1, basic G2 and Q1 in SPEC.
            Depends on: none; implementation requires accepted/integrated documentation and eligible phase activation.
            Acceptance/evidence: Pin compatible tooling; validate type/build/unit commands and dedicated Chromium/Firefox binaries with two context/page/interaction/rendering/teardown cycles each. Follow browser diagnostics/fallback policy with finite deadlines. Keep affected work blocked on required failures; no false playable-game claim.
        - [ ] T-002 — Implement board geometry and baseline piece source
            Scope: src/domain/types.ts, geometry.ts, landing.ts, source.ts; tests/unit and board/source fixtures. Contracts: G1, basic G2 and Q1 in SPEC.
            Depends on: T-001.
            Acceptance/evidence: Verify seven matrices, rotations without kicks, bounds/overlap, regular landing, deterministic source consumption and rejected movement; pure code has no browser imports.
        - [ ] T-003 — Integrate a focused falling-piece game page
            Scope: src/domain/game.ts, src/app/controller.ts, src/browser/input.ts, clock.ts, board.ts, src/main.ts, styles and index; focused integration/E2E. Contracts: G1, basic G2 and Q1 in SPEC.
            Depends on: T-002.
            Acceptance/evidence: Built page starts a piece, renders cells and accepts focused lateral/rotation/soft-drop plus timed descent. Invalid moves preserve stack; first visible-play browser smoke passes. Lock/clear completion belongs to the next milestone.
            Milestone exit: after all other milestone tasks (T-001, T-002) complete, perform whole-capability code review, required tests/regressions and blocker repair. Commit milestone report and HANDOFF with this task result and initial checkbox; publish/read back before issue/milestone closure.
            Report: docs/dev/reports/phases/1/1.1.md.
    - [ ] Milestone 1.2 — Complete classic round
        - [ ] T-004 — Implement full-interval legal locking
            Scope: src/domain/locking.ts, game.ts and time/landing fixtures. Contracts: G1, G3 interval and G4 score in SPEC.
            Depends on: T-003.
            Acceptance/evidence: Observe first-grounding deadline, no movement resets, chronological equal-time events, airborne expiry and atomic in-bounds non-overlap placement. Test exact boundary times with explicit clock input.
        - [ ] T-005 — Implement simultaneous clears scoring and levels
            Scope: src/domain/geometry.ts, game.ts, panel status and tests. Contracts: G1, G3 interval and G4 score in SPEC.
            Depends on: T-004.
            Acceptance/evidence: Verify 1–4 simultaneous clears, row compaction, scoring/soft-drop points, cumulative lines, level thresholds and gravity formula without exposing partial board state.
        - [ ] T-006 — Complete top-out pause and restart lifecycle
            Scope: controller/game/input/clock, native lifecycle controls and integration/E2E checks. Contracts: G1, G3 interval and G4 score in SPEC.
            Depends on: T-005.
            Acceptance/evidence: Verify blocked spawn, game-over actions, pause/focus/hidden suspension, resume without catch-up, restart state reset and listener/loop uniqueness; demonstrate a complete classic round.
            Milestone exit: after all other milestone tasks (T-004, T-005) complete, perform whole-capability code review, required tests/regressions and blocker repair. Commit milestone report and HANDOFF with this task result and initial checkbox; publish/read back before issue/milestone closure.
            Report: docs/dev/reports/phases/1/1.2.md.
    - [ ] Milestone 1.3 — Baseline delivery
        - [ ] T-007 — Finish readable panel keyboard help and focus behavior
            Scope: src/browser/panel.ts, board.ts, input.ts, styles, docs/USER-GUIDE.md. Contracts: Q1 and A1/A3/A7 baseline subset in SPEC.
            Depends on: T-006.
            Acceptance/evidence: Display score/lines/level/status and controls; verify keyboard repeat timing, native-control focus protection, visible focus and restart access in both browsers.
        - [ ] T-008 — Verify the built classic game in both browsers
            Scope: tests/e2e/game.spec.ts, integration fixtures and built-server runner. Contracts: Q1 and A1/A3/A7 baseline subset in SPEC.
            Depends on: T-007.
            Acceptance/evidence: Exercise A1 and baseline A3 on built assets in Chromium/Firefox; inspect Canvas output, clear/top-out, focus, repeated lifecycle and teardown. Repair required failures; record executable/mode and evidence limits.
        - [ ] T-009 — Prepare reproducible baseline package and release notes
            Scope: package metadata, README, USER-GUIDE/DEVELOPMENT, RELEASE-NOTES.md and build exclusions. Contracts: Q1 and A1/A3/A7 baseline subset in SPEC.
            Depends on: T-008.
            Acceptance/evidence: Prepare 0.1.0 and complete truthful notes; clean reproducible static build loads without external runtime dependencies. Validate documented CMD/Linux commands and built acceptance. Publication waits for phase integration.
            Milestone exit: after all other milestone tasks (T-007, T-008) complete, perform whole-capability code review, required tests/regressions and blocker repair. Commit milestone report and HANDOFF with this task result and initial checkbox; publish/read back before issue/milestone closure.
            Report: docs/dev/reports/phases/1/1.3.md.
    - [ ] Milestone 1.4 — Phase review
        - [ ] T-010 — Review test and report phase 1
            Depends on: T-009 and completion/verified hosted closure of every delivery milestone (1.1, 1.2, 1.3). Its own review milestone remains open until this task completes.
            Scope/evidence: review the complete phase code and cross-milestone interactions; run included SPEC acceptance, prior regressions, required Chromium/Firefox built-app and package checks. Resolve required defects, verify 0.1.0 notes/source agreement, and commit PHASE-REPORT and HANDOFF with this checkbox. Do not claim subsequent integration or release publication in task evidence.
            Report: docs/dev/reports/phases/1/PHASE-REPORT.md.

## Phase 2 — Modern play

- [ ] Phase 2 — Modern play
    - [ ] Milestone 2.1 — Piece source and rotation
        - [ ] T-011 — Implement bag and queue consumption
            Scope: src/domain/source.ts, game.ts and source fixtures. Contracts: G2 and A2 source/rotation in SPEC.
            Depends on: T-010.
            Acceptance/evidence: Each aligned bag has one of each kind, exhaustion/refill works and preview/lock consumption is deterministic; establish shuffle boundary/random sample checks.
        - [ ] T-012 — Implement ordered wall and floor kicks
            Scope: src/domain/geometry.ts, game.ts and rotation fixtures. Contracts: G2 and A2 source/rotation in SPEC.
            Depends on: T-011.
            Acceptance/evidence: Verify clockwise/counterclockwise transforms, O no-op, first-success offset order, wall/floor cases and complete rejection with unchanged timer/stack. No SRS claim.
        - [ ] T-013 — Integrate source and rotation acceptance
            Scope: game/controller, tests/integration and E2E rotation/source scenarios, developer/user docs. Contracts: G2 and A2 source/rotation in SPEC.
            Depends on: T-012.
            Acceptance/evidence: Demonstrate source boundary and focused wall/floor rotation on the built game; preserve top-out, lock/clear and baseline regressions. Fixtures do not expose production debug controls.
            Milestone exit: after all other milestone tasks (T-011, T-012) complete, perform whole-capability code review, required tests/regressions and blocker repair. Commit milestone report and HANDOFF with this task result and initial checkbox; publish/read back before issue/milestone closure.
            Report: docs/dev/reports/phases/2/2.1.md.
    - [ ] Milestone 2.2 — Hold and previews
        - [ ] T-014 — Implement Hold transitions and usage guard
            Scope: src/domain/game.ts and Hold/source fixtures. Contracts: G4 Hold and G2 source consumption in SPEC.
            Depends on: T-013.
            Acceptance/evidence: Empty Hold consumes one next kind; occupied swap consumes none. Reset incoming pose/timing, reject repeated Hold and handle blocked incoming spawn through game over.
        - [ ] T-015 — Render Hold and next previews with input controls
            Scope: src/browser/panel.ts, input.ts, projection values and focused E2E. Contracts: G4 Hold and G2 source consumption in SPEC.
            Depends on: T-014.
            Acceptance/evidence: Show stored/next kinds, map C/Shift, reflect usage guard and verify no double action from key repeat or focused form controls.
        - [ ] T-016 — Verify integrated Hold and source lifecycle
            Scope: controller integration, E2E fixtures and user guide. Contracts: G4 Hold and G2 source consumption in SPEC.
            Depends on: T-015.
            Acceptance/evidence: Verify lock re-enables Hold, restart clears it, swaps retain exact queue order and all classic actions remain playable; demonstrate both preview panels without affecting source.
            Milestone exit: after all other milestone tasks (T-014, T-015) complete, perform whole-capability code review, required tests/regressions and blocker repair. Commit milestone report and HANDOFF with this task result and initial checkbox; publish/read back before issue/milestone closure.
            Report: docs/dev/reports/phases/2/2.2.md.
    - [ ] Milestone 2.3 — Landing aids
        - [ ] T-017 — Implement shared ghost projection
            Scope: src/domain/landing.ts, projection.ts, board.ts and fixtures. Contracts: G3 hard drop and G4 ghost in SPEC.
            Depends on: T-016.
            Acceptance/evidence: Always-visible Phase 2 ghost agrees with regular terminal landing across roofs, floor and obstructed wells; visually distinguish ghost without mutating state.
        - [ ] T-018 — Implement delayed hard drop and drop scoring
            Scope: game/input/locking and focused time/source tests. Contracts: G3 hard drop and G4 ghost in SPEC.
            Depends on: T-017.
            Acceptance/evidence: Space repositions to the same query as ghost, awards two points per row and retains/starts interval deadline as specified; zero-distance repeats neither score nor reset.
        - [ ] T-019 — Verify modern play and prepare release
            Scope: built E2E, README/guide, package version and RELEASE-NOTES.md. Contracts: G3 hard drop and G4 ghost in SPEC.
            Depends on: T-018.
            Acceptance/evidence: Verify A2 and applicable A3/A7 in both browsers, including Hold→drop and blocked rotation. Prepare reproducible 0.2.0 package/notes; configuration alternatives remain deferred.
            Milestone exit: after all other milestone tasks (T-017, T-018) complete, perform whole-capability code review, required tests/regressions and blocker repair. Commit milestone report and HANDOFF with this task result and initial checkbox; publish/read back before issue/milestone closure.
            Report: docs/dev/reports/phases/2/2.3.md.
    - [ ] Milestone 2.4 — Phase review
        - [ ] T-020 — Review test and report phase 2
            Depends on: T-019 and completion/verified hosted closure of every delivery milestone (2.1, 2.2, 2.3). Its own review milestone remains open until this task completes.
            Scope/evidence: review the complete phase code and cross-milestone interactions; run included SPEC acceptance, prior regressions, required Chromium/Firefox built-app and package checks. Resolve required defects, verify 0.2.0 notes/source agreement, and commit PHASE-REPORT and HANDOFF with this checkbox. Do not claim subsequent integration or release publication in task evidence.
            Report: docs/dev/reports/phases/2/PHASE-REPORT.md.

## Phase 3 — Session configuration

- [ ] Phase 3 — Session configuration
    - [ ] Milestone 3.1 — Configurable rule behavior
        - [ ] T-021 — Define settings schema defaults and availability
            Scope: src/settings/schema.ts, controller and settings fixtures. Contracts: C1, G3 alternatives and G4 dynamic in SPEC.
            Depends on: T-020.
            Acceptance/evidence: Validate exact six setting domains/defaults; hard-drop/Hold/preview availability changes preserve board/source and held kind. No persistence yet.
        - [ ] T-022 — Implement captured lock-policy alternatives
            Scope: game/locking/controller and timing fixtures. Contracts: C1, G3 alternatives and G4 dynamic in SPEC.
            Depends on: T-021.
            Acceptance/evidence: Implement failed-down versus interval and immediate versus follow hard drop. Current piece retains captured selections; next spawn/Hold uses changed preferences. Test grounded changes and repeated drops.
        - [ ] T-023 — Implement midline ghost and combined option rules
            Scope: projection/landing, settings/controller fixtures and docs. Contracts: C1, G3 alternatives and G4 dynamic in SPEC.
            Depends on: T-022.
            Acceptance/evidence: Dynamic hides only if every projected cell is above row 10; straddling is visible. Verify hide/show, ghost with hard drop off and retained Hold usage across off/on.
            Milestone exit: after all other milestone tasks (T-021, T-022) complete, perform whole-capability code review, required tests/regressions and blocker repair. Commit milestone report and HANDOFF with this task result and initial checkbox; publish/read back before issue/milestone closure.
            Report: docs/dev/reports/phases/3/3.1.md.
    - [ ] Milestone 3.2 — Session settings panel
        - [ ] T-024 — Build the six native settings controls
            Scope: src/browser/panel.ts, styles and E2E settings focus checks. Contracts: C1 and A5 in SPEC.
            Depends on: T-023.
            Acceptance/evidence: All allowed choices/defaults have labels, focus indicators and clear next-piece lock feedback. Gameplay keys cannot alter play while editing controls.
        - [ ] T-025 — Integrate live options across session states
            Scope: controller/panel/input and integration/E2E fixtures. Contracts: C1 and A5 in SPEC.
            Depends on: T-024.
            Acceptance/evidence: Ready/running/paused/over changes act at specified time; disabled previews/actions and Hold preservation work; restart keeps selected options while resetting session.
        - [ ] T-026 — Verify option interactions and prepare release
            Scope: tests/e2e/game.spec.ts and settings.spec.ts, guide/package/notes. Contracts: C1 and A5 in SPEC.
            Depends on: T-025.
            Acceptance/evidence: Exercise all values and material lock/drop/Hold/ghost combinations in both browsers; preserve prior acceptance and prepare 0.3.0 package/notes without claiming reload persistence.
            Milestone exit: after all other milestone tasks (T-024, T-025) complete, perform whole-capability code review, required tests/regressions and blocker repair. Commit milestone report and HANDOFF with this task result and initial checkbox; publish/read back before issue/milestone closure.
            Report: docs/dev/reports/phases/3/3.2.md.
    - [ ] Milestone 3.3 — Phase review
        - [ ] T-027 — Review test and report phase 3
            Depends on: T-026 and completion/verified hosted closure of every delivery milestone (3.1, 3.2). Its own review milestone remains open until this task completes.
            Scope/evidence: review the complete phase code and cross-milestone interactions; run included SPEC acceptance, prior regressions, required Chromium/Firefox built-app and package checks. Resolve required defects, verify 0.3.0 notes/source agreement, and commit PHASE-REPORT and HANDOFF with this checkbox. Do not claim subsequent integration or release publication in task evidence.
            Report: docs/dev/reports/phases/3/PHASE-REPORT.md.

## Phase 4 — Penetrator gameplay

- [ ] Phase 4 — Penetrator gameplay
    - [ ] Milestone 4.1 — Penetrator rules
        - [ ] T-028 — Implement deepest legal penetrator landing
            Scope: src/domain/landing.ts, geometry.ts and roof/pocket fixtures. Contracts: G5 landing/transit and G3 interactions in SPEC.
            Depends on: T-027.
            Acceptance/evidence: Select greatest legal row at/below candidate through occupied rows; reject no-landing cases. Distinguish regular first obstruction and prove settled-board invariance.
        - [ ] T-029 — Implement penetrator transit and candidate validity
            Scope: game/geometry and transition fixtures. Contracts: G5 landing/transit and G3 interactions in SPEC.
            Depends on: T-028.
            Acceptance/evidence: Down/gravity traverse occupied cells; moves/kicks may cross occupancy but remain in bounds with a valid terminal landing. Reject invalid candidates atomically without changing pose, source or clock.
        - [ ] T-030 — Integrate terminal grounding and safe lock timing
            Scope: game/locking and explicit-time fixtures. Contracts: G5 landing/transit and G3 interactions in SPEC.
            Depends on: T-029.
            Acceptance/evidence: Ignore intermediate pockets/support, start deadline at terminal support, allow one successful activation reset, prohibit later resets and never lock overlapping transit cells. Test failed-down and airborne-expiry modes.
        - [ ] T-031 — Unify penetrator ghost and hard drop
            Scope: projection/game/landing and controller integration checks. Contracts: G5 landing/transit and G3 interactions in SPEC.
            Depends on: T-030.
            Acceptance/evidence: Both consumers query identical deepest landing under all ghost/drop/lock options; immediate and delayed drops place only legal cells; invariant failure ends play without writing stack.
        - [ ] T-032 — Review test and report milestone 4.1
            Depends on: T-028, T-029, T-030, T-031; all delivery results verified and published.
            Scope/evidence: whole Penetrator rules code review and applicable contract/integration/regression tests; fix blockers, update HANDOFF and commit report with this task checkbox before closure. Keep non-critical eligible deferrals with provenance/options.
            Report: docs/dev/reports/phases/4/4.1.md.
    - [ ] Milestone 4.2 — Earned bonus experience
        - [ ] T-033 — Implement four-clear awards and activation spending
            Scope: game/controller/input, bonus/source/Hold fixtures. Contracts: G5 bonuses/appearance and A4 in SPEC.
            Depends on: T-032.
            Acceptance/evidence: Award exactly one per four-clear capped at three; B spends once only for valid activation; reject zero/repeated activation, disable Hold and preserve source/balance on failure.
        - [ ] T-034 — Render white pieces and live bonus symbols
            Scope: board/panel/projection, styles and visual E2E. Contracts: G5 bonuses/appearance and A4 in SPEC.
            Depends on: T-033.
            Acceptance/evidence: All active/locked penetrator cells are white, all regular kinds nonwhite; show one lightning symbol plus accessible balance per bonus and update on earning/spend.
        - [ ] T-035 — Verify earned penetration and prepare release
            Scope: integration/E2E fixtures, guide/package/notes. Contracts: G5 bonuses/appearance and A4 in SPEC.
            Depends on: T-034.
            Acceptance/evidence: Demonstrate earning then activation, roof traversal and legal locking in both browsers with Hold disabled. Cover selected policies, invisible ghost, cap/repeat/failure and prior regressions. Prepare 0.4.0 package/notes.
            Milestone exit: after all other milestone tasks (T-033, T-034) complete, perform whole-capability code review, required tests/regressions and blocker repair. Commit milestone report and HANDOFF with this task result and initial checkbox; publish/read back before issue/milestone closure.
            Report: docs/dev/reports/phases/4/4.2.md.
    - [ ] Milestone 4.3 — Phase review
        - [ ] T-036 — Review test and report phase 4
            Depends on: T-035 and completion/verified hosted closure of every delivery milestone (4.1, 4.2). Its own review milestone remains open until this task completes.
            Scope/evidence: review the complete phase code and cross-milestone interactions; run included SPEC acceptance, prior regressions, required Chromium/Firefox built-app and package checks. Resolve required defects, verify 0.4.0 notes/source agreement, and commit PHASE-REPORT and HANDOFF with this checkbox. Do not claim subsequent integration or release publication in task evidence.
            Report: docs/dev/reports/phases/4/PHASE-REPORT.md.

## Phase 5 — Persistent configuration

- [ ] Phase 5 — Persistent configuration
    - [ ] Milestone 5.1 — Independent resolution
        - [ ] T-037 — Implement pure per-setting resolution
            Scope: src/settings/resolve.ts, schema.ts and settings fixtures. Contracts: C2 read/resolve in SPEC.
            Depends on: T-036.
            Acceptance/evidence: Validate each key independently using URL→cookie→default; cover every allowed value, empty/invalid/case/encoding/duplicates and unknown keys without cross-setting suppression.
        - [ ] T-038 — Implement bounded cookie and query reading
            Scope: src/browser/persistence.ts and persistence integration tests. Contracts: C2 read/resolve in SPEC.
            Depends on: T-037.
            Acceptance/evidence: Read current origin cookies/query defensively through ports, apply first URL/last cookie duplicate rules and catch unavailable cookie access without crashing play.
        - [ ] T-039 — Integrate startup selection and explicit URL saves
            Scope: controller/persistence and E2E startup tests. Contracts: C2 read/resolve in SPEC.
            Depends on: T-038.
            Acceptance/evidence: Save valid explicit URL selections including defaults; omitted/invalid parameters retain valid cookies. Verify mixed-source startup in both browsers without populating omitted query keys.
            Milestone exit: after all other milestone tasks (T-037, T-038) complete, perform whole-capability code review, required tests/regressions and blocker repair. Commit milestone report and HANDOFF with this task result and initial checkbox; publish/read back before issue/milestone closure.
            Report: docs/dev/reports/phases/5/5.1.md.
    - [ ] Milestone 5.2 — Persistent settings experience
        - [ ] T-040 — Persist changes and replace shareable URL
            Scope: persistence/controller, schema encoders and integration checks. Contracts: C2 writes/failure and A6 in SPEC.
            Depends on: T-039.
            Acceptance/evidence: Every selected change writes its cookie with scope/lifetime/security attributes and replaces only its parameter without reload/history growth; preserve path, fragment, unrelated parameters and other cookies.
        - [ ] T-041 — Handle unavailable persistence destinations
            Scope: persistence/panel/controller and failure-port tests. Contracts: C2 writes/failure and A6 in SPEC.
            Depends on: T-040.
            Acceptance/evidence: Cookie exceptions/unobservable writes and history failure retain usable in-memory configuration and surface destination status. No frame retry loop or rollback of successful destination; explicit changes may retry.
        - [ ] T-042 — Verify reload matrix and prepare complete delivery
            Scope: E2E settings/game suites, guides/README, version/package/notes. Contracts: C2 writes/failure and A6 in SPEC.
            Depends on: T-041.
            Acceptance/evidence: Verify all six allowed-value reloads, explicit defaults, omitted-cookie retention, mixed sources, duplicates and URL preservation in Chromium/Firefox plus full-game regressions. Prepare 0.5.0 static package and accurate full-feature notes.
            Milestone exit: after all other milestone tasks (T-040, T-041) complete, perform whole-capability code review, required tests/regressions and blocker repair. Commit milestone report and HANDOFF with this task result and initial checkbox; publish/read back before issue/milestone closure.
            Report: docs/dev/reports/phases/5/5.2.md.
    - [ ] Milestone 5.3 — Phase review
        - [ ] T-043 — Review test and report phase 5
            Depends on: T-042 and completion/verified hosted closure of every delivery milestone (5.1, 5.2). Its own review milestone remains open until this task completes.
            Scope/evidence: review the complete phase code and cross-milestone interactions; run included SPEC acceptance, prior regressions, required Chromium/Firefox built-app and package checks. Resolve required defects, verify 0.5.0 notes/source agreement, and commit PHASE-REPORT and HANDOFF with this checkbox. Do not claim subsequent integration or release publication in task evidence.
            Report: docs/dev/reports/phases/5/PHASE-REPORT.md.
            Final report: docs/dev/reports/IMPLEMENTATION-REPORT.md; aggregate unresolved/deferred milestone and phase TODOs with original IDs, impact, options, rationale, owner and resolution references. Verify complete A1–A7 scope before claiming full delivery.

