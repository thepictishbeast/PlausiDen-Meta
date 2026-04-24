# Exceptions — Protocol and Template

Per [`OPERATING_PRINCIPLES.md`](OPERATING_PRINCIPLES.md) §12: when a consumer
project cannot meet a PlausiDen doctrine requirement, the answer is a
**formal exception**, not silent deviation. This file is the protocol.

## When exceptions are and aren't appropriate

**Appropriate**:

- A specific legacy code path where the doctrine can't yet be satisfied but the retirement plan is clear.
- A third-party integration that forces a documented deviation (e.g., an upstream library that doesn't support the required crypto primitive; fix landing in upstream's next release).
- A platform-level constraint (e.g., an embedded target lacks the required logging substrate; limited alternative declared).
- A time-bounded experiment where a doctrine tenet is temporarily relaxed to gather evidence (must cite the evidence goal).

**Not appropriate**:

- **Preference.** "We don't like this rule." File a doctrine amendment instead.
- **Convenience.** "Following the rule is hard." The cost of compliance is the *point* of the rule; convenience-based exceptions undermine doctrine.
- **Scope too broad.** "Our entire codebase" / "all Rust code we ship" / "everything under `src/`." Reduce scope until you can name it file-by-file or reject the exception as a doctrine disagreement.
- **No expiry.** "Indefinitely" / "until further notice" / "TBD." Every exception dies on a named date.
- **Non-waivable tenet.** If the doctrine marks a tenet `waivable = false`, no exception exists. Either comply, fork, or don't adopt PlausiDen.

## Mandatory fields

Every exception ships a markdown file at `exceptions/<exception-id>.md` in
the consumer's own repo (not in any PlausiDen repo) with these fields:

```markdown
# Exception: <short-description>

**Exception ID**: `<stable-id>`
**Consumer repo**: `<consumer/repo>`
**Declared**: <ISO-8601 date>
**Expires**: <ISO-8601 date — MANDATORY, no open-ended exceptions>
**Reviewed on**: <next review date, must be ≤ expiry>

## Excepted tenet

**Doctrine**: `<doctrine-repo>` version `<doctrine-version>`
**Tenet ID**: `<exact tenet id from doctrine/principles.toml>`
**Tenet text**: "<copy the tenet text verbatim>"
**Waivable?**: yes  # must be yes; non-waivable tenets cannot be excepted

## Scope

**File(s) / module(s) / endpoint(s)**: <exact glob or fully-qualified name>
**Lines** (if applicable): <line range or specific function signatures>
**Environment** (if applicable): dev | staging | production | all
**NOT covered**: <explicitly list what this exception does NOT permit>

## Justification

<Technical reason the doctrine cannot be met in this scope. Concrete, not preferential.>

## Plan to retire

**Blocking condition**: <what must change for this exception to be removable>
**Expected resolution path**: <the work required, linked to an issue>
**Work-tracking ticket**: <issue tracker URL — mandatory>

## Approver

- Consumer maintainer: <name, date>
- Cross-project (if doctrine-breaking at ecosystem level): <PlausiDen-Meta maintainer, date>

## Mitigation

<What the consumer does instead of full doctrine compliance. May reduce the
severity of the risk the doctrine protects against; document the residual
risk explicitly.>

## Audit trail

- <ISO date> — exception filed
- <ISO date> — <any status updates: review, scope change, expiry extension with justification>
```

## If the template doesn't fit on one page

The exception is too broad. Split it into multiple smaller exceptions or
reject it as requiring a doctrine amendment.

## Expiry extension protocol

An exception may be extended **once**, via a formal amendment to the
exception file adding an audit-trail entry and bumping the expiry date.
Extension requires:

- Fresh justification (not "we didn't finish yet").
- Fresh plan-to-retire with concrete milestones.
- Explicit approver sign-off on the extension, with reason.

A second extension requires the exception be re-filed from scratch with
fresh review — at which point the underlying issue is either resolved or
escalated to a doctrine amendment proposal.

## Expired exceptions

An exception whose expiry date has passed automatically demotes to a
doctrine violation. CI fails; merge is blocked; the scope must be
brought into compliance or a fresh exception filed (with a fresh
review). There is no grace period.

## Audit-by-PlausiDen-Meta

`PlausiDen-Audits` (when its engine ships) scans consumer `exceptions/`
directories as part of every audit run:

- Counts exceptions per consumer, by doctrine, by tenet.
- Flags exceptions approaching expiry (warn at 14 days; error at 3 days).
- Flags exceptions missing mandatory fields.
- Flags exceptions whose scope has grown since first filing (comparing git history).

## Cross-project and ecosystem-wide exceptions

If a consumer's exception would set a precedent for the ecosystem (e.g.,
"this doctrine tenet is unimplementable on embedded targets"), the
exception must be filed to `PlausiDen-Meta` as well as the consumer's own
repo, and may precipitate a doctrine amendment. The `PlausiDen-Meta`
maintainer reviews; if a pattern emerges across consumers, the amendment
process (see [`GOVERNANCE.md`](GOVERNANCE.md)) converts the pattern to
formal doctrine.

## The meta-rule

**If you find yourself arguing for an exception, also draft the
corresponding amendment.** Exceptions are evidence that doctrine may need
refinement. The amendment may succeed or fail — either way, the two
documents (exception + amendment draft) make the ecosystem smarter.
Filing an exception without engaging the amendment process is
intellectually dishonest and degrades doctrine over time.
