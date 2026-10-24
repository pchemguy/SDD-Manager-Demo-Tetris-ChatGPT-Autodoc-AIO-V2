# Gameplay contracts

This child owns G1–G5 under [SPEC](../SPEC.md). Coordinates are integer column x (0–9) and row y (0–19), with y increasing downward. A pose combines kind, orientation and matrix origin. Only occupied cells count for bounds/overlap; padding does not.

## Board, pieces and lifecycle

G1. The board has 10 columns and 20 visible rows, with no hidden playable rows. Each settled cell is empty or records a tetromino kind and penetrator appearance. Spawn orientations use the following matrices, where dots are padding; rows are separated by slashes.

| Kind | Spawn matrix | Regular color |
| --- | --- | --- |
| I | `..../IIII/..../....` | Cyan |
| O | `OO/OO` | Yellow |
| T | `.T./TTT/...` | Purple |
| J | `J../JJJ/...` | Blue |
| L | `..L/LLL/...` | Orange |
| S | `.SS/SS./...` | Green |
| Z | `ZZ./.ZZ/...` | Red |

Spawn at y=0, x=floor((10−matrix width)/2), orientation zero. All occupied cells must be inside the visible board and clear. Blocked spawn ends the game without modifying the stack. No action can lock above the board. Regular pieces retain nonwhite colors; locked penetrator cells retain their white appearance until cleared.

States are ready, running, paused and game over. Enter or Start starts ready play. Restart is explicit through the restart button or Enter after game over; it empties the board, score/lines/bonuses/Hold, resets level to 1 and creates a fresh source, retaining selected settings. During running, Enter does not silently discard a game. P or Pause toggles running/paused; invalid state actions are no-ops. Game over accepts restart and settings only.

Lock is atomic: validate the active cells, place them, remove every full row simultaneously, compact surviving rows downward preserving order, insert empty rows at top, update score/lines/level/bonuses, reset Hold availability and spawn the next piece. No frame exposes partially cleared cells. If the next spawn fails, the completed placement/clear remains and status becomes game over. State corruption that prevents legal placement ends play without writing overlapping cells.

## Movement, rotation and piece source

G2. Left/Right attempts one column. Down attempts one row and is soft drop. Successful player soft-drop descent earns one point per row; blocked movement earns none. Up or X rotates clockwise; Z rotates counterclockwise. O rotation leaves its pose unchanged. For I, rotate the 4×4 matrix about its center; J/L/S/T/Z use their 3×3 matrix centers. Four clockwise rotations return the exact original occupied cells before kicks.

For each non-O rotation, test the rotated pose using offsets `(0,0),(-1,0),(1,0),(-2,0),(2,0),(0,-1),(0,-2)` in this order; accept the first valid result. If none is valid, preserve the whole prior pose. This is a defined simple kick system, with no claim of SRS compatibility. Regular validity includes bounds and settled-cell collision. Penetrator validity is defined under G5. Movement/rotation cannot change or erase settled cells.

The final piece source uses shuffled bags containing exactly one of each kind. Random samples are in [0,1); use an unbiased Fisher–Yates shuffle with uniform samples. Exhaust one bag before consuming the next. One next-piece preview shows the next unconsumed kind when enabled. Hiding the preview does not alter the sequence. A normal lock consumes one next piece. First Hold consumes one; swapping an occupied Hold consumes none. Rejected actions consume none.

Space performs hard drop when enabled. C or Shift attempts Hold; B attempts penetrator activation. Ignore native repeated keydown events. The app repeats held Left/Right after 150 ms and every 50 ms thereafter; most recently pressed direction wins while both are held. Down repeats every 50 ms. Rotation, Hold, activation, pause and hard drop occur once per press. Clearing focus/visibility clears repeat state; paused input cannot queue actions for resumption.

## Gravity and locking

G3. At level L, gravity interval is max(80, floor(1000 × 0.8^(L−1))) milliseconds. Gravity attempts one downward row per interval of running simulation time. A newly spawned piece starts a fresh gravity interval. Soft drop and hard drop do not add elapsed simulation time. Process elapsed events chronologically, with lock expiry before a gravity attempt at the same timestamp. Stop advancing when game over/paused; never carry stale deltas across suspension.

A regular piece is grounded when its current pose is legal and a one-row downward move is blocked. A penetrator is grounded only at its lowest legal terminal landing, even if intermediate occupied cells are encountered. Successful movement that reaches support establishes grounding immediately, without waiting for another failed Down.

Each spawned piece captures general policy and hard-drop policy from selected settings. Under interval policy, first grounding starts one deadline using the then-current gravity interval. Movement, rotation, failed Down, and repeated hard drop never renew the deadline. It continues if a movement/kick makes the piece airborne. At expiry, lock if grounded; otherwise retain the expired deadline and lock on its next legal grounding. Never lock in transit or while airborne. Pause/hidden time is excluded. No unlimited reset count exists.

Under failed-down policy, a failed gravity or player Down attempt at a legal grounded position locks immediately. Merely arriving at support does not lock until a failed attempt. Other failed actions do not lock.

Hard drop moves to the queried landing and adds two points per descended row. Immediate policy then locks in that action. Follow policy applies general locking: interval policy starts a deadline only if none exists; an expired deadline locks at grounding, while an existing future deadline is retained. Failed-down policy waits for a later failed Down/gravity attempt; the positioning action is not itself that attempt. A zero-distance drop earns zero and never renews a deadline. When hard drop is disabled, Space has no gameplay effect.

## Score, Hold and ghost

G4. A simultaneous clear of 1/2/3/4 rows scores 100/300/500/800 times the level immediately before the clear. Add soft/hard-drop points independently. Lines is the cumulative number of cleared rows; level=1+floor(lines/10). No combo, spin, back-to-back or other scoring bonus applies.

Hold is enabled by default. The first successful Hold stores the active kind and spawns the next; later swaps store the outgoing kind and spawn the held kind. Incoming pieces use spawn orientation/position and fresh piece timing/policy capture. One Hold is allowed between locks; unsuccessful/disabled Hold does not consume it. Blocked incoming spawn ends play through normal top-out. Disabling Hold preserves its stored kind and usage restriction. Hold is unavailable while penetrator mode is active.

The ghost uses the same landing query as hard drop, irrespective of whether hard drop is enabled. Always-hide draws none; always-show draws the queried landing, possibly covered by an active piece at the same location. Dynamic hides only if every projected occupied cell has y<10, otherwise shows it. A landing spanning rows 9 and 10 is visible; one ending in row 9 is hidden. Use projected cells, not origin, active position or average height. No landing means no ghost. It remains visually distinct from the active piece; rendering a ghost does not change collisions or score.

## Penetrator

G5. A simultaneous four-row clear awards one bonus after placement, with balance=min(3,balance+1); other clears award none. The balance starts at zero and one lightning symbol is shown per bonus, including zero as no symbols plus accessible numeric text. Activation requires running status, a normal active piece, positive balance and a legal terminal landing; otherwise it is a state-preserving no-op. Success spends exactly one, makes all active cells white and lasts until lock. Repeated B does not spend again; there is no deactivation or refund. Hold cannot act while active.

Define terminal landing by examining every origin row at or below the candidate pose while keeping its x/orientation: choose the greatest row for which all occupied cells are inside the board and do not overlap settled cells. Ignore occupied cells along the downward path. Reject a candidate with no such row. Regular landing instead stops before the first obstruction; it cannot use a deeper pocket behind blockers.

An activated penetrator advances under gravity/Down one row at a time through occupied cells without destroying or moving them. A horizontal move/rotation may cross occupied cells, but its candidate occupied cells must stay within the board and admit a terminal landing at or below that candidate. It is grounded only at that terminal landing; higher clear pockets do not ground it. All moves/rotations are atomic, using G2 kick order with these candidate rules.

Activation discards any earlier normal-piece lock deadline because terminal grounding changes; capture the interval when first reaching the penetrator terminal landing. Captured locking-policy selection remains the piece's spawn-time selection. This reset occurs once per successful activation, which spends a bonus; movement and repeated activation cannot reset it. A penetrator's deadline may expire during later transit but cannot place an overlapping piece. At the next valid terminal grounding it can lock under G3.

Hard drop and ghost both use this deepest terminal landing. Immediate hard-drop locking still requires a legal final pose. If a violated internal invariant unexpectedly produces no legal final pose, end the game without altering settled cells. Locked penetrators follow ordinary simultaneous line clear/scoring and may earn a four-clear bonus. They have no continuing ability after placement.

## Focused acceptance fixtures

| Case | Expected result |
| --- | --- |
| Ground at t=200 with interval 1000; slide at t=900 | No lock at 1199; lock at 1200 if grounded. |
| Same timer expires airborne at 1200; ground at 1300 | No airborne lock; immediate lock at 1300. |
| Pause at 700, resume after 5000 wall ms | Deadline consumes only running time; no catch-up drop. |
| Regular and penetrator over a roof with a lower empty pocket | Regular stops above roof; penetrator preview/drop uses deepest legal position below it; roof cells unchanged. |
| Penetrator overlapping stack during transit | No placement or line clear until legal terminal landing; board bytes unchanged by transit. |
| No legal terminal position for activation or kick | Preserve piece, timer, queue and bonuses. |
| Balance 2, activate twice, then four-clear | Balance changes 2→1→1→2; Hold rejected while active. |
| Balance 3 with four-clear | Remains 3. |
| Hold empty, repeated Hold, lock, swap | Consume one queued kind, reject repeated Hold, restore availability on lock, consume no kind on swap. |
| Dynamic landing cells all above row 10 / one at row 10 | Hidden / visible, irrespective of active pose height. |
| Interval hard drop repeated at same pose | First valid deadline retained; no additional points. |

Use legal handcrafted boards and explicit time/source inputs. Scenario assertions include board, active cells, queue consumption, score, status and deadline where relevant, rather than checking only a rendered label.
