# Configuration contracts

This child owns C1–C2 under [SPEC](../SPEC.md). The selected setting is separate from the captured locking policy of an already active piece.

## Schema and live behavior

C1. Use the same stable key for each URL parameter and its separate cookie. Match keys and values case-sensitively with no whitespace coercion.

| Key | Allowed serialized values | Default | Effect |
| --- | --- | --- | --- |
| `ghost` | `hide`, `dynamic`, `show` | `dynamic` | Change ghost visibility immediately. |
| `hardDrop` | `on`, `off` | `on` | Enable/ignore Space immediately; ghost calculation remains available. |
| `next` | `on`, `off` | `on` | Show/hide next preview immediately without consuming source. |
| `hardDropLock` | `follow`, `immediate` | `follow` | Capture on next spawned piece. |
| `lock` | `failedDown`, `interval` | `interval` | Capture on next spawned piece. |
| `hold` | `on`, `off` | `on` | Allow/ignore Hold immediately; retain held kind and usage restriction. |

Every setting has a labeled native select or equivalent accessible control in the side panel. Show all choices with understandable text. Hide or visibly disable unavailable gameplay controls/previews consistently. The penetrator restriction overrides `hold=on`; it does not rewrite the saved preference. Changing hard-drop locking while hard drop is disabled still saves the choice for later use.

Settings can change while ready, running, paused or over. Changes do not reset score, board, timing or source. Explain that locking selections affect the next piece. Hold-spawned pieces capture current locking selections, but the one-Hold-per-lock rule still applies. Restart retains selected settings and captures them in its first piece.

## Persistence resolution and failures

C2. On page load, resolve every key separately: first a valid explicitly present URL value, else a valid cookie, else default. Save each valid explicit URL value to its corresponding cookie even if it equals the default. Omitted or invalid URL values do not overwrite a valid saved cookie with a default. An invalid cookie is treated as absent. URL selection for one key never suppresses cookie selection for another.

Use URLSearchParams percent decoding and its first value for duplicate known keys. Empty, unknown, differently cased or unrecognized known-key values are invalid. Malformed encoded strings that do not decode to an allowed value are invalid. Ignore unknown keys. Cookies use the last occurrence of an exact known key if duplicates are exposed by the browser; decode percent encoding defensively, treating decoding errors as invalid. Cookies and query strings have no authority to execute code or add new settings.

Each valid player change updates selected in-memory settings immediately, writes its own cookie and updates its own URL parameter using history.replaceState without page reload or a new history entry. Preserve path, fragment and unrelated query parameters. Replace duplicate entries for the changed key with one value. Leave omitted other known parameters and their cookies intact. Startup need not populate the URL with cookie/default values or normalize invalid query text; it must persist valid explicit URL values. A change that selects the already-selected value still performs the required write/update.

Write cookies with Path=/, SameSite=Lax and Max-Age=31536000 (one year); add Secure on HTTPS. Encode value bytes using encodeURIComponent-compatible serialization. Never save board/session data or credentials. Read/write only these six application cookies. Opening the game without an explicit query selection retains saved cookies on the same origin/path scope.

Treat denied cookie access, a write not observable on readback, and history.replaceState failure as persistence unavailable for the affected destination. Catch exceptions; retain selected in-memory values and playable behavior. Report a concise status that a setting may not survive reload or its shareable URL could not update. Do not repeatedly retry on animation frames. A later explicit settings change can attempt its required writes again. Successful destination writes need not be rolled back because the other destination failed.

For session-only delivery, all C1 behavior and defaults remain usable without claiming reload persistence. The final product satisfies C2. Local-file browser cookie behavior is not a required delivery mode; run static assets from an HTTP(S) origin for acceptance.

## Acceptance matrix

| Inputs / action | Expected result |
| --- | --- |
| URL `ghost=hide`; cookie `ghost=show` | Select hide and save hide cookie. |
| URL omits ghost; cookie show | Select show; do not replace cookie with dynamic. |
| URL `ghost=dynamic`; cookie hide | Select dynamic and save it even though default. |
| URL invalid ghost; cookie show | Select show; do not persist invalid value or default over it. |
| Invalid URL and invalid cookie for one key | Use that key's default; other keys resolve independently. |
| URL supplies hardDrop=off; cookies supply hold=off and ghost=show | All three take effect independently. |
| Query duplicates `ghost=hide&ghost=show` | First value hide wins at startup; changing to dynamic leaves one ghost parameter. |
| Player changes next only | Update next cookie/parameter; retain other cookie values, query parameters and fragment. |
| Cookie or History API fails | In-memory change still works; visible persistence status; no game reset/crash. |
| Lock policy changes while grounded | Current piece retains captured policy/deadline; next spawned piece uses new selection. |
| Reload after every allowed setting value | Restore that value by the specified precedence, including explicit defaults. |

Boundary tests cover every allowed serialized value, omitted/empty/invalid values, duplicate keys, percent encoding and mixed sources. Browser tests verify real reload behavior in both required browsers. Pure parser tests alone do not prove cookie/history integration.
