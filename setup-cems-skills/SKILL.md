---
name: setup-cems-skills
description: Configure this repo for the CEMS engineering skills — writes issue tracker, triage labels, and domain doc config. Run once before first use of the other engineering skills.
disable-model-invocation: true
---

# Setup CEMS Skills

Scaffold the per-repo configuration that the engineering skills assume. No questions are asked — CEMS conventions are fixed:

- **Issue tracker** — always local markdown under `.scratch/`
- **Triage labels** — always the five standard roles (unchanged)
- **Domain docs** — always single-context (`CONTEXT.md` + `docs/adr/` at the repo root)

## Process

### 1. Explore

Check the current state — read, don't assume:

- `CLAUDE.md` at the repo root — does it exist? Does it already have an `## Agent skills` section?
- `docs/agents/` — do config files already exist there?

### 2. Git repository check

Check whether `.git/` exists at the repo root.

If it does **not** exist, skip to step 3.

If it **does** exist:

1. Check whether `.scratch/` is already listed in `.gitignore` (if that file exists).
2. Ask the user one question:

   > This is a git repository. Should the local issue tracker folder (`.scratch/`) be **synchronised** (tracked by git and committed) or **ignored** (added to `.gitignore` so it stays local-only)?

3. Act on the answer:
   - **Synchronised** — do nothing; `.scratch/` will be tracked as-is.
   - **Ignored** — append `.scratch/` to `.gitignore` (create the file if it does not exist). If it was already present, confirm that to the user and skip writing.

### 3. Write

Write all files directly. No confirmation step.

**`docs/agents/issue-tracker.md`** — copy from [issue-tracker-local.md](./issue-tracker-local.md).

**`docs/agents/triage-labels.md`** — copy from [triage-labels.md](./triage-labels.md).

**`docs/agents/domain.md`** — copy from [domain.md](./domain.md).

**`CLAUDE.md`** — add or update the `## Agent skills` section with this exact content:

```
## Agent skills

### Issue tracker

Issues live as local markdown files under `.scratch/<feature>/`. See `docs/agents/issue-tracker.md`.

### Triage labels

Standard five-role vocabulary (needs-triage, needs-info, ready-for-agent, ready-for-human, wontfix). See `docs/agents/triage-labels.md`.

### Domain docs

Single-context repo — one `CONTEXT.md` and `docs/adr/` at the root. See `docs/agents/domain.md`.
```

Rules for editing `CLAUDE.md`:

- If `CLAUDE.md` does not exist, create it with only the `## Agent skills` block.
- If `CLAUDE.md` exists but has no `## Agent skills` section, append the block.
- If `## Agent skills` already exists, update it in-place. Don't overwrite surrounding sections.

If `docs/agents/` files already exist, overwrite them — CEMS conventions are fixed and re-running is intentional.

### 4. Done

Report which files were written or updated. Mention that `docs/agents/*.md` can be edited manually for fine-tuning, but re-running this skill will overwrite them.
