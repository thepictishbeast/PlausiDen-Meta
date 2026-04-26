# Dynamic Cleaner Schema (v0.1)

The contract that **PlausiDen-Atrium** consumes from **PlausiDen-Tidy**,
**PlausiDen-AppGuard**, and any future cleaner-emitting tool.

Atrium is a fork of [BleachBit](https://www.bleachbit.org/) (GTK + Python,
GPL-3.0+). It keeps BleachBit's mature backend (200+ stock cleaners,
cross-platform path handling, dry-run, localization, edge-case fixes) and
adds a `DynamicCleaner` class that loads JSON files matching this schema
and presents them in the same tree-view UI as the stock cleaners.

Tidy + AppGuard never ship a UI. They emit JSON to a watched directory;
Atrium auto-refreshes its tree.

## Why this exists

- **Atrium UI** — keep BleachBit's UX; don't reinvent
- **Atrium backend** — keep BleachBit's stock cleaners (browsers, system
  caches, app-specifics) — don't replicate months of work
- **Tidy + AppGuard** — Rust-first deterministic rule engines that emit
  cleaners as data, no UI
- **Schema = the seam** — third-party emitters plug in for free

## Watch directory

Atrium polls (via inotify when available) the directory:

```
/var/lib/atrium/dynamic-cleaners/*.json
```

Each `.json` file is one cleaner. Files are owned by the user of the
emitter (Tidy, AppGuard) and group-readable by the `atrium` group.
Atrium reloads its dynamic cleaners on any file change.

(For per-user installs, the watched dir is `~/.local/share/atrium/dynamic-cleaners/`.)

## Schema (v0.1)

```json
{
  "schema_version": "0.1.0",
  "id": "<unique-stable-id>",
  "label": "<short, shown in tree-view>",
  "description": "<full prose, shown in detail pane>",
  "category": "<group label, e.g. 'PlausiDen Tidy' / 'PlausiDen AppGuard' / 'My Org'>",
  "danger": "low | medium | high",
  "estimated_recovery_bytes": <integer or null>,
  "actions": [
    {
      "type": "delete | truncate | shred | wipe-free-space",
      "path": "<absolute path or glob>",
      "predicate": {
        "size_min_mb": <int or null>,
        "atime_older_than_days": <int or null>,
        "mtime_older_than_days": <int or null>,
        "owner": "<user|root|null>",
        "type": "file | directory | any"
      },
      "preview_only": <bool>
    }
  ],
  "requires_confirmation": <bool>,
  "default_selected": <bool>,
  "emitted_by": "<tool name>@<version>",
  "emitted_at": "<ISO 8601 timestamp>",
  "rationale": "<one-line explanation of WHY this should be cleaned>",
  "rollback_hint": "<text for if-things-go-wrong, optional>"
}
```

### Required fields

`schema_version`, `id`, `label`, `category`, `danger`, `actions`, `emitted_by`, `emitted_at`.

### Field semantics

- **`id`** — stable across emissions (Atrium uses it to track user's
  per-cleaner enable/disable state). Convention: `<emitter>-<rule-name>`,
  e.g. `tidy-mail-spool-overgrown`, `appguard-zoom-unused-180d`.

- **`category`** — Atrium groups cleaners by this string in the tree.
  Reserve `"PlausiDen Tidy"` and `"PlausiDen AppGuard"`; third parties
  use their own.

- **`danger`** — UI-only signal. Atrium colors high-danger entries red,
  requires double-confirm.

- **`estimated_recovery_bytes`** — best-effort estimate. Used to sort the
  tree and show "would recover ~X GB" in the status bar.

- **`actions[].type`** —
  - `delete` — remove file or directory. Subject to `predicate`.
  - `truncate` — open + truncate to zero. Preserves inode + permissions.
  - `shred` — multi-pass overwrite (theatrical on SSD/COW; emit advisory).
  - `wipe-free-space` — fill free space with zeros, then delete.

- **`actions[].predicate`** — only act when ALL non-null conditions hold.
  (Atrium re-checks predicates at execution time, in case state changed
  since the JSON was emitted.)

- **`actions[].preview_only`** — if true, the action shown in BleachBit's
  preview pane is informational only and won't actually execute.

- **`requires_confirmation`** — if true, Atrium prompts even when the
  user has bulk-approved a category.

- **`default_selected`** — if true, the cleaner is checked by default in
  the tree-view (use sparingly; conservative default is false).

- **`rollback_hint`** — text Atrium shows in a "if you regret this" toast
  after action. Example: "If you needed those: `apt-get install --reinstall <pkg>`".

## Examples

### Tidy — overgrown mail spool

```json
{
  "schema_version": "0.1.0",
  "id": "tidy-mail-spool-overgrown",
  "label": "Mail spool over 100MB",
  "description": "Truncate /var/mail/<user> when size exceeds 100 MB. Common cause: cron jobs whose stdout/stderr never gets read. Truncate preserves the inode so existing mail readers don't lose track.",
  "category": "PlausiDen Tidy",
  "danger": "low",
  "estimated_recovery_bytes": 8589934592,
  "actions": [
    {
      "type": "truncate",
      "path": "/var/mail/*",
      "predicate": { "size_min_mb": 100, "type": "file" },
      "preview_only": false
    }
  ],
  "requires_confirmation": false,
  "default_selected": true,
  "emitted_by": "tidy@0.1.0",
  "emitted_at": "2026-04-26T18:00:00Z",
  "rationale": "Cron-spam mail accumulates unbounded; only the first few KB are useful for diagnosis.",
  "rollback_hint": "If you needed those messages: they were probably cron job stdout — re-run the job to capture again."
}
```

### AppGuard — unused app

```json
{
  "schema_version": "0.1.0",
  "id": "appguard-zoom-unused-180d",
  "label": "Zoom (not launched in 180+ days, 938 MB)",
  "description": "Zoom desktop client has not been launched in 180 days. Binary at /opt/zoom; configs at ~/.zoom and ~/.cache/zoom. Total disk: 938 MB.",
  "category": "PlausiDen AppGuard",
  "danger": "low",
  "estimated_recovery_bytes": 983789568,
  "actions": [
    { "type": "delete", "path": "/opt/zoom", "predicate": { "type": "directory" }, "preview_only": false },
    { "type": "delete", "path": "/usr/share/applications/Zoom.desktop", "predicate": { "type": "file" }, "preview_only": false },
    { "type": "delete", "path": "/home/*/.zoom", "predicate": { "type": "directory" }, "preview_only": false },
    { "type": "delete", "path": "/home/*/.cache/zoom", "predicate": { "type": "directory" }, "preview_only": false }
  ],
  "requires_confirmation": true,
  "default_selected": false,
  "emitted_by": "appguard@0.1.0",
  "emitted_at": "2026-04-26T18:00:00Z",
  "rationale": "Last launch detected 188 days ago via .desktop launch hook + binary atime. App size is non-trivial.",
  "rollback_hint": "Reinstall via your package manager or download from zoom.us if needed later."
}
```

## Versioning

The `schema_version` field follows semver. Atrium accepts any version
that matches the major version it was built against. Tidy + AppGuard
declare the schema version they emit; Atrium logs (but tolerates)
patch-version mismatches.

When the schema needs a breaking change:
1. Bump major version
2. Atrium adds a parser for the new version (keep old parser alive)
3. Emitters can target either
4. Drop old parser after 90 days of zero observed v(N-1) emissions

## What this schema deliberately does NOT do

- **No predicates beyond file metadata** — no "if user hasn't opened X
  in Y days" inside the predicate. Emitters compute that ahead of time
  and embed the result in `description` + the binary decision to emit
  or not. Keeps Atrium's predicate checker dumb + fast.
- **No actions beyond filesystem ops** — no service stop/start, no
  network calls, no privilege changes. If a cleaner needs more, the
  emitter writes a wrapper script + invokes that as `delete`-of-a-marker.
- **No conditional chains / depends-on** — each cleaner is independent.
  If you need ordering, encode it in two cleaners with shared category
  + UI sort hints.

## Roadmap

| Version | What |
|---|---|
| 0.1 | This document — initial schema |
| 0.2 | Add `i18n` block (per-locale label/description) |
| 0.3 | Add `dry_run_command` for a CLI override (e.g. `du -sh path`) |
| 1.0 | Production stability commitment + 90-day deprecation cycle |
