# Repository layout

## Scope and existing assets

This layout allocates the logical owners in [DECOMPOSITION](DECOMPOSITION.md) and the increments in [PLAN](PLAN.md). Product source, tests and package files below are intended locations, not claims that those files already exist. Existing root policies, `.agents/` plugin/skill assets and manual provenance scaffolding remain in place.

| Location | Physical owner and purpose | Corresponding checks |
| --- | --- | --- |
| `src/domain/types.ts` | Shared browser-free value contracts for board, pose, actions, state and snapshots | Type checking and consumer/domain tests |
| `src/domain/geometry.ts` | Matrices, transforms, bounds/overlap, placement and row compaction | `tests/unit/geometry.test.ts` |
| `src/domain/landing.ts` | Regular and penetrator terminal queries and grounding predicates | `tests/unit/landing.test.ts` |
| `src/domain/source.ts` | Uniform baseline and final bag/queue source | `tests/unit/source.test.ts` |
| `src/domain/locking.ts` | Captured policy/deadline calculations | `tests/unit/locking.test.ts` |
| `src/domain/game.ts` | Session transitions, score/level, Hold, penetrator balance and lifecycle coordination | `tests/unit/game.test.ts`, `tests/integration/game-flow.test.ts` |
| `src/domain/projection.ts` | Read-only presentation/ghost derivation | `tests/unit/projection.test.ts` |
| `src/settings/schema.ts` | Six-setting types, stable keys, allowed values/defaults and validation | `tests/unit/settings.test.ts` |
| `src/settings/resolve.ts` | Pure query/cookie parsing and independent precedence | `tests/unit/settings.test.ts` |
| `src/app/controller.ts` | Session/configuration orchestration and snapshot distribution | `tests/integration/controller.test.ts` |
| `src/browser/input.ts`, `src/browser/clock.ts` | Keyboard/focus/repeat and animation/suspension adapters | Controller tests and `tests/e2e/game.spec.ts` |
| `src/browser/board.ts`, `src/browser/panel.ts` | Canvas presentation and native panel/settings controls | E2E rendering, focus and control assertions |
| `src/browser/persistence.ts` | Cookie/URL ports and destination-failure outcomes | `tests/integration/persistence.test.ts`, `tests/e2e/settings.spec.ts` |
| `src/main.ts`, `src/styles.css`, `index.html` | Composition root, static shell and styles | Build and actual built-app E2E |
| `tests/fixtures/` | Small legal boards, piece streams and timing traces; no production backdoors | Unit/integration fixture consumers |
| `tests/e2e/` | Real built-game Chromium/Firefox interaction, visual and reload coverage | Pinned Playwright runner/config |
| `package.json`, lockfile, `tsconfig.json`, build/unit/Playwright configs | Pinned development tooling, script entry points and reproducible static build | Type/build/unit/integration/browser checks |
| `README.md`, `docs/USER-GUIDE.md`, `docs/DEVELOPMENT.md` | Current user instructions, setup/check commands and browser evidence limits | Link and command checks with owning tasks |
| `RELEASE-NOTES.md` | Complete current release message, source/version/limitations | Phase/release verification; matching GitHub body and release commit |
| `dist/`, `test-results/`, `.browser-runtime/` | Generated assets, evidence and dedicated browser installation/state | Ignore generated paths; never commit browser binaries |
| `docs/dev/` | Canonical project/design/SPEC/PLAN/layout/TASKS and preparation/QC records | Owning document reviews |
| `docs/dev/reports/phases/<phase-id>/` | Milestone `<milestone-id>.md` and `PHASE-REPORT.md` | Assigned milestone/phase boundary tasks |
| `docs/dev/reports/IMPLEMENTATION-REPORT.md` | Final implementation evidence and TODO aggregation | Last phase-review task |
| `docs/ai-logs/` | Manual historical conversation records under its own AGENTS | Consult only for requested provenance |

## Placement and dependencies

Use separate files for independently testable rule areas, keeping helpers local when they share one purpose. `game.ts` coordinates transitions rather than duplicating matrix transforms or terminal search. Domain types cannot import application/browser modules. The controller imports domain and pure settings values; browser adapters receive narrow callbacks/ports. The composition root connects them. Browser APIs remain in `src/browser/` and composition code; no cookie or DOM import enters the domain or resolver.

Tests mirror behavioral owners rather than private function names. Integration tests use real domain logic with controlled time/random/storage ports. Browser tests exercise built assets from an HTTP origin and real controls. Place expected boards/streams in fixtures; no debug gameplay commands, seeded bonus controls or arbitrary stack editing are exposed in production UI solely to facilitate tests.

Use a dedicated browser runtime under the ignored `.browser-runtime/` or an equivalently identified job-owned directory; set the same absolute location for installation and all test processes. Ignore Playwright state/results and build output as they are introduced. Keep secrets excluded by existing `*.tkn`; no code or packaging step reads them.

## Navigation and command ownership

Root AGENTS links the canonical document owners and continuity record. README explains delivered capability and links user/developer docs as those are created. Developer instructions own validated package scripts and actual platform/browser setup evidence. No command is described as validated before it is exercised. Exact tooling versions/config filenames are selected and pinned during the first implementation task; architecture does not require an unverified package version.

Source/test allocation supports the five planned increments without moving components between phases. Implementation may introduce a cohesive helper at an existing owner when needed; a changed boundary or new public contract requires updating its governing documents and affected QC. TASKS owns concrete expected edit scope and progress; layout contains no executable checklist.
