# Delivery plan

## Strategy and release boundaries

Deliver five bounded playable increments, preserving the preceding working game. [SPEC](SPEC.md) owns complete G1–G5, C1–C2 and Q1 contracts; [ARCHITECTURE](ARCHITECTURE.md) and [DECOMPOSITION](DECOMPOSITION.md) own structure. Later contracts are scheduled honestly rather than required at every earlier exit.

| Phase | Outcome and version | Dependency | Scope boundary |
| --- | --- | --- | --- |
| 1 — Classic baseline | Playable classic game, 0.1.0 | None | G1, basic G2, G3 interval policy, G4 score, applicable Q1. No Hold, ghost, kicks, hard drop, bag, settings or bonuses. |
| 2 — Modern play | Familiar piece flow and landing aids, 0.2.0 | Verified, integrated Phase 1 | Complete G2 and G4 Hold/ghost/drop; G3 delayed hard drop. Ghost always shown and hard drop follows interval policy until settings arrive. |
| 3 — Session configuration | Six working controls and dynamic ghost, 0.3.0 | Verified, integrated Phase 2 | Complete C1 and G3 policy alternatives; dynamic default. Persistence C2 and penetrator G5 remain deferred. |
| 4 — Penetrator gameplay | Earn, spend and play penetrator bonuses, 0.4.0 | Verified, integrated Phase 3 | Complete G5 with Hold/drop/ghost/policy interactions; settings remain session only. |
| 5 — Persistent configuration | Reloadable/shareable preferences, 0.5.0 | Verified, integrated Phase 4 | Complete C2 and full-product cross-contract acceptance. No session-board persistence. |

The classic baseline uses independent uniformly selected kinds and collision-rejected rotation; Phase 2 provides final bag and kick behavior. These scoped deferrals keep Phase 1 a genuinely small playable product. Phase 3's two cohesive milestones are preferable to separate releases for each control; Phase 4 isolates the substantial transit/locking rules; Phase 5 isolates storage/URL failure modes. Version 0.5.0 covers the complete intended feature scope without promising a stable 1.x interface. Compatible fixes increment patch versions.

## Phase 1 — Classic baseline

The first useful product supports an entire game: start, move/rotate, gravity, legal lock, row clearing, scoring/level, top-out, pause and restart. Full-interval locking is included because it is the requested baseline default; modern conveniences are not necessary to play.

| Milestone | Integrated outcome and exit evidence |
| --- | --- |
| 1.1 — Falling-piece play | A built page accepts focused movement/rotation and shows active and settled cells. Establish pinned TypeScript/build/unit/browser tooling and representative isolated Chromium/Firefox execution early. Geometry/source/clock seams support repeatable checks. Demonstrate spawn/movement and visible Canvas before expanding rules. |
| 1.2 — Complete classic round | Add legal deadline locking, simultaneous row clear, scoring/levels, top-out and pause/restart. Demonstrate a deterministic clear and complete round; verify no overlap, event-order boundaries and no suspended-time catch-up. |
| 1.3 — Baseline delivery | Finish readable panel/help/focus behavior, built-app browser acceptance and reproducible static packaging/developer instructions. Prepare version and release notes for 0.1.0; verify A1, A3 and applicable A7. |
| 1.4 — Phase review | One phase code review/testing/report outcome after all delivery milestones complete/close. Assess complete baseline usefulness and regressions; authorize no extra capabilities by implication. |

The falling-piece milestone gives early feedback on legibility/input; the complete round exposes lock/clear usability before packaging. Evidence supports human decisions to continue, amend, simplify or stop under the authorized execution range. It does not impose an extra approval round when continuation is already authorized.

## Phase 2 — Modern play

| Milestone | Integrated outcome and exit evidence |
| --- | --- |
| 2.1 — Piece source and rotation | Replace independent selection with tested 7-bag consumption and add the defined ordered kicks. Demonstrate bag boundary and wall/floor rotation while preserving top-out and board invariants. |
| 2.2 — Hold and previews | Add Hold, once-per-lock behavior and next/Hold panels. Demonstrate empty/swap/blocked Hold, correct queue consumption and preserved scoring/locking. |
| 2.3 — Landing aids | Share regular landing between ghost and hard drop; add drop input/points and full-interval follow policy. Demonstrate projection/drop equality, repeat-drop no-reset and resumed input. Prepare 0.2.0 notes/package. |
| 2.4 — Phase review | One code review/testing/report outcome covering integrated modern play and prior classic acceptance; A2 and applicable A3/A7. |

These capabilities form a coherent modern keyboard-play increment. Immediate hard-drop locking and visibility selection stay with Phase 3, avoiding an unfinished configuration surface. Landing-aid evidence gives a clear point to assess readability and lock-delay feel.

## Phase 3 — Session configuration

| Milestone | Integrated outcome and exit evidence |
| --- | --- |
| 3.1 — Configurable rule behavior | Establish the six-setting schema/defaults, failed-down/immediate/follow policies, capture semantics and ghost midline rules. Verify these in domain/controller traces, including straddling rows 9/10 and grounded policy changes. |
| 3.2 — Session settings panel | Integrate all six labeled controls, immediate visibility/availability behavior, preserved Hold state and next-piece policy feedback. Demonstrate changes during ready/running/paused/over states, focus separation and session-only restart behavior. Prepare 0.3.0 package/notes. |
| 3.3 — Phase review | One code review/testing/report outcome for A5 and all applicable A1–A3/A7 regressions; confirm no persistence claim. |

Two delivery milestones are retained because behavior and its usable control surface are the two meaningful integration risks; adding administrative milestones would fragment a narrow increment. Persistence is independent and remains later. Inspect control clarity and next-piece feedback at the panel milestone.

## Phase 4 — Penetrator gameplay

| Milestone | Integrated outcome and exit evidence |
| --- | --- |
| 4.1 — Penetrator rules | Implement deepest legal landing, traversal, candidate rejection and terminal grounding/locking with deterministic fixtures. Verify roof/pocket paths, intermediate overlaps, no-landing rejection and immutable settled cells. Shared ghost/drop queries must agree. |
| 4.2 — Earned bonus experience | Integrate four-clear awards/cap, activation/spend, Hold restriction, white piece/locked cells and bonus symbols. Demonstrate earning and spending through actual play plus fixtures; exercise every selected lock/drop/ghost option. Prepare 0.4.0 package/notes. |
| 4.3 — Phase review | One code review/testing/report outcome for A4 and complete prior regressions, especially source consumption and board legality. |

Two delivery milestones keep algorithmic and earned-use risks separately inspectable within one useful phase; neither is a separate promised release. Penetrator rules are not required to play earlier releases. Bonus/visual integration provides human evidence to assess the ability's clarity and usefulness.

## Phase 5 — Persistent configuration

| Milestone | Integrated outcome and exit evidence |
| --- | --- |
| 5.1 — Independent resolution | Integrate validated per-setting URL/cookie/default selection, malformed/duplicate handling and explicit-default saves. Demonstrate mixed-source startup and retained omitted cookies. |
| 5.2 — Persistent settings experience | Add cookie writes, URL replacement and degradation status; verify all six choices across reloads, unrelated URL preservation and unavailable destinations. Finish user/developer docs and 0.5.0 packaging/notes. |
| 5.3 — Phase review | One code review/testing/report outcome for A6 and full A1–A7. Include final implementation report and aggregate remaining eligible deferred TODOs with provenance/options; required defects block completion. |

Two delivery milestones match reading/precedence and writing/reload behavior; a third would add no independent product outcome. The reload/share demonstration supports assessment of the finished configuration experience.

## Verification and integration exits

Each delivery milestone requires whole-capability code review, focused tests/regressions, repair of required defects, a committed milestone report and HANDOFF update. TASKS assigns a standalone milestone review only when both thresholds hold: at least four delivery tasks in that milestone and at least two delivery milestones in its phase. Otherwise its final delivery task owns those obligations and depends on the other milestone tasks. Every special final phase-review milestone has exactly one review/testing/report task and is excluded from delivery counts.

Phase review waits until all delivery milestones and their issues are complete/closed, then checks cross-milestone contracts, browser evidence, static packaging and remaining findings. It has its own report and closes its own milestone afterward. Pure test success does not replace code review. A required browser, rendering, contract or packaging failure blocks the affected exit; do not downgrade coverage to make it pass. Plan checks for real input, screenshots/shape assertions, repeat contexts/teardown and built-app reloads under browser policy. Run deterministic unit/integration checks alongside each change.

Before completing a phase, verify every included contract, prior working behavior, current document/checklist evidence and hosted associations/closures. Integrate the complete phase through an explicit two-parent merge into the actual default branch, verify the merged state and publish/read back. Partial task ranges pause on their phase branch. Activate only an authorized next phase after predecessor integration; preparation does not create future implementation issues.

After each verified phase integration, finalize its matching version and complete release notes, establish the release commit on the actual default branch, verify message/source coverage, push/read back, create and verify its tag, and publish/read back the GitHub release. Prepare release files before phase review; release publication follows integration and does not pretend to have occurred in a task commit. A blocked release is retained as pending and is resolved before phase advancement. Version/tag/body must agree. Preserve historical tags rather than move them.

## Risks and readiness

Browser provisioning is the first implementation readiness risk; diagnose Chromium and Firefox separately with finite deadlines, trusted managed binaries and actual repeated page/interaction/rendering checks. No browser installation is required for this plan. Avoid global tooling changes and preserve CMD-compatible developer commands.

Lock-event ordering and penetrator overlap are the main rule risks; explicit time/board fixtures and one landing owner address them. Persistence failures are isolated from in-memory play. Physical placement is owned by layout; TASKS consumes reviewed strategy and layout together. No architecture or product decisions remain blocked at this planning boundary.
