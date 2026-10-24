# Browser Tetris

## Purpose and outcomes

Build a browser-based, single-player Tetris game for keyboard play. Deliver a useful classic game first, then bounded playable increments covering the complete scope below. The finished game combines familiar controls with configurable play and an earned penetrator ability.

## Complete scope

The classic baseline includes falling tetrominoes, lateral movement, rotation, gravity, locking, line clearing, scoring, levels and increasing speed, game over and restart. Pause/resume supports ordinary desktop play.

Enhancements include Hold, ghost projection, 7-bag randomization, wall kicks and hard drop. The default locking policy waits one complete gravity interval after grounding; hard drop follows that policy unless immediate hard-drop locking is selected.

Ghost visibility supports always hidden, dynamic and always shown. Dynamic visibility depends on the projected landing position relative to the board midline.

A four-line clear earns one penetrator bonus, capped at three. A hotkey spends a bonus to activate the current piece until it locks. The piece can traverse occupied cells without changing them, but must lock entirely inside the board without overlap. Its hard drop and ghost use the lowest legal landing reachable through occupied cells. Active penetrators are white; regular pieces are never white. The side panel displays one symbol per available bonus and updates on earning/spending.

The side panel controls ghost visibility, hard-drop availability, next preview, hard-drop locking, general locking and Hold. Every setting persists independently through its cookie and URL parameter. Valid explicit URL values take precedence over cookies, then defaults; explicit defaults are saved too. Omitted parameters retain saved values. Player changes update both cookie and URL. Session-only configuration is a useful intermediate increment.

## Proposed gameplay decisions

These recommendations are submitted for approval with this brief; they are not prior user decisions. SPEC will define precise contracts and acceptance examples.

| Concern | Recommended behavior and reason |
| --- | --- |
| Board and midline | Use 10 columns and 20 visible rows. Dynamic ghost is hidden only when all projected cells are in the upper ten rows. A ghost straddling the midline remains visible, making partial entry into the lower half unambiguous. |
| Lock timing | Start the timer on first grounding; movement/rotation never renews it. It continues while temporarily airborne, but expiry cannot lock an airborne piece: lock on the next legal grounding. Pause and hidden-tab suspension freeze simulation time. |
| Speed changes | Capture the gravity interval when a grounding timer starts, so changing speed cannot shorten a running timer unexpectedly. |
| Hard drop | Reposition immediately and apply the selected lock policy. Dropping an already grounded piece does not renew its timer; immediate mode locks at once. |
| Penetrator and Hold | Disable Hold while the penetrator is active. Spend a bonus once on activation; repeated activation does nothing. No manual deactivation or refund. Ordinary Hold is available at most once between locks. |
| Penetrator landing and locking | Use the lowest fully legal position at the current orientation and horizontal location. Intermediate overlaps never lock. Penetrator gravity can traverse occupied rows; its terminal grounding and timer occur at that lowest landing. If activation or a proposed move/rotation leaves no legal terminal position, reject it without spending a bonus or changing the piece. |
| Invalid penetrator state | If an unexpected state has no legal lock position, end the game without committing overlapping cells; never destroy stack cells to repair it. |
| Failed-down policy | A failed player or gravity downward attempt at a legal terminal landing locks. Passing through occupied cells is not a failed attempt for a penetrator. |
| Configuration changes | Apply visibility and availability changes immediately. Disabling Hold preserves its stored piece. Apply lock-policy changes to the next spawned piece, keeping the current piece’s lock behavior stable; detailed timing belongs in SPEC. |
| Invalid persisted values | Treat an invalid URL value as absent for that setting, then try its cookie/default; never persist malformed values. Invalid cookies fall back to defaults. Preserve unrelated URL parameters. |
| Defaults | Dynamic ghost, hard drop enabled, next preview enabled, Hold enabled, full-interval general locking, hard drop follows general policy. |

## Delivery and constraints

Recommend TypeScript with Canvas for the board and ordinary HTML controls for settings and status. This keeps gameplay logic testable while providing native focusable configuration controls. A DOM-only board is feasible but adds element-level rendering work; a UI framework offers little benefit for this small standalone game. These are proposed implementation choices, not constraints supplied by the user.

Target desktop keyboard use in Chromium and Firefox, following BROWSER_DEV_POLICY.md with dedicated managed browser environments. Browser provisioning and actual execution evidence are implementation prerequisites; no browser capability is claimed by document preparation. Preserve Windows CMD compatibility for developer commands under WINDOWS_SHELL_POLICY.md; the current preparation host is Linux.

Use a deterministic game core and injected randomness/time for repeatable tests. Deliver as static browser assets with no backend. Proposed exclusions are multiplayer, server leaderboards, accounts, mobile touch controls and persistence of an in-progress board.

Choose semantic versions beginning with 0.1.0 for the verified classic baseline, incrementing minor versions for useful capability increments and patch versions for compatible fixes. Publish a tagged GitHub release after each completed, verified and integrated implementation phase. PLAN owns exact phase/version allocation; releases represent delivered and verified behavior.

## Success criteria

The baseline is playable without later enhancements. Every increment preserves earlier behavior, has observable acceptance evidence and can be released independently. Final acceptance covers all requested gameplay, all six settings and per-setting precedence, including malformed values and reloads. Ghost and hard drop agree for regular and penetrator pieces, and no lock can overlap occupied cells.
