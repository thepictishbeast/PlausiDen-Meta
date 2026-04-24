# Contributor Checklist

> Run through this **before** opening a PR against any PlausiDen-namespace
> repo. Applies equally to humans, Claude Code sessions, other AI agents,
> and external contributors.
>
> Most fragmentation comes from skipping step 1. Don't skip step 1.

---

## Step 1 — Search before adding

Before introducing **anything** new (rule, contract, harness, template,
doctrine clause, vocabulary field, principle, anti-pattern, audit,
example), search the relevant repo:

```sh
# Adapt the search root to the repo you're contributing to.
REPO=/path/to/PlausiDen-<X>

# 1. Full-text search for keywords related to your contribution
grep -rn "<keyword>" "$REPO"/

# 2. Read the existing doctrine end-to-end
cat "$REPO"/DOCTRINE.md "$REPO"/doctrine/*.toml

# 3. List existing artifacts
ls -R "$REPO"/{audits,contracts,templates,examples,doctrine,tiers,scripts}/ 2>/dev/null

# 4. Read README + any *.md at repo root
ls "$REPO"/*.md && cat "$REPO"/README.md
```

**If you find an existing artifact that's 80% of what you wanted to add,
extend it.** Do not introduce a parallel artifact.

## Step 2 — Independence test

Apply the test from [`SCOPE.md`](SCOPE.md):

> If a stranger cloned this repo tomorrow and knew nothing about my project, would this artifact make complete sense as a standalone?

If yes → continue.
If no → generalize the artifact OR move it to your own consumer repo.

Common contamination patterns to grep for and remove:

- Specific consumer names (LFI, Sacred.Vote, Protection Suite, etc.) in normative content. The "Known consumers" section of [`REPO_LABEL_REGISTRY.md`](REPO_LABEL_REGISTRY.md) is the only acceptable place to name them.
- Specific URLs to your own infrastructure (your repo's GitHub, your VPS, your domain).
- Specific tooling assumptions ("uses Hetzner," "pin Tailscale," "deploys to Vercel") in normative content.
- Specific language assumptions where the doctrine is meant to be language-neutral.
- Examples that only work for your stack — restate them with a `<placeholder>` or use a generic example.

## Step 3 — Doctrine alignment

Reference the relevant doctrine tenet by id:

- For a new audit rule: cite which Auditing Doctrine tenet it implements.
- For a new test pattern: cite which Testing Doctrine tenet it satisfies.
- For a new vocabulary field: confirm it doesn't overlap with existing fields.
- For a new principle: cite the concrete object-level pain it eliminates per [`OPERATING_PRINCIPLES.md`](OPERATING_PRINCIPLES.md) §10.

If your contribution doesn't align with an existing doctrine clause, you
have two paths:

- **Path A**: drop the contribution. The doctrine doesn't sanction it; it's not yours to add.
- **Path B**: file a doctrine amendment first, get it ratified, then file the contribution that implements it.

Never commit a contribution that depends on an unratified doctrine change.

## Step 4 — Cross-repo coordination

If your contribution touches more than one PlausiDen repo (e.g., a Canon
contract that requires a new Audits rule and a new Tests harness), open
an issue in [`PlausiDen-Meta`](https://github.com/thepictishbeast/PlausiDen-Meta)
first to coordinate the cross-repo amendment.

Otherwise you risk shipping the Canon contract on Tuesday and the
matching Audits rule on Thursday, with consumers in between blocked.

## Step 5 — Specificity scope

Per [`OPERATING_PRINCIPLES.md`](OPERATING_PRINCIPLES.md) §13, contributions
must not be over-specific:

- **Per-consumer specifics belong in the consumer's repo.** Not in PlausiDen.
- **Per-stack specifics belong in `adapters/<stack>/`** within the appropriate PlausiDen repo. Not in the spec.
- **Per-tool specifics belong in `templates/<tool>/`**. Not in the doctrine.
- **Per-environment specifics belong in `examples/`**. Not in the rule body.

If your contribution feels too specific to live where you're putting it,
it is. Move it to a more specific layer or back to your consumer's repo.

## Step 6 — Author the PR

PR description includes:

- **Search results from step 1** — confirm you didn't find a duplicate. (One sentence: "Searched `audits/` for `<keyword>` — no existing match.")
- **Independence test result** — confirm the artifact passes. (One sentence: "A stranger cloning this repo would understand X without knowing Y.")
- **Doctrine clause referenced** — id of the tenet your contribution implements or extends.
- **Cross-repo touch list** — repos affected; coordination issue link if applicable.
- **Specificity scope** — explicit declaration of where the artifact lives and why that level.

PRs missing any of these get closed with a one-line "see CONTRIBUTOR_CHECKLIST.md."

## Step 7 — Co-author trailer

Every commit includes a `Co-Authored-By:` trailer naming the contributing
agent or human. Agent contributions explicitly identify the model/version.

```
Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
```

---

## When this checklist is overkill

Trivial fixes (typos, broken links, missing punctuation, formatting): skip
to step 6 and just open the PR. The checklist is for *additive* contributions.

When in doubt, run the checklist.
