---
name: smart-commit
description: Reviews uncommitted git changes, writes a commit strategy with conventional commit messages, and asks the user for approval before applying commits. Use when the user wants to commit changes, asks to "commit this", "write a commit", "stage and commit", or wants help structuring uncommitted work into proper commits.
---

# Smart Commit

## Quick start

1. Check git is available — if not, stop and report it. Do not attempt workarounds.
2. Run `git status` and `git diff` (staged + unstaged) to see all pending changes.
3. Write a commit strategy.
4. Present the strategy to the user and ask for approval.
5. Apply only after the user confirms.

## Workflow

### Step 1 — Verify git

```bash
git --version
```

If this fails or git is not on PATH: tell the user git is unavailable and stop. Do not try to install git or find alternatives.

### Step 2 — Collect changes

Run these in parallel:

```bash
git status --short
git diff HEAD          # all changes vs last commit (staged + unstaged)
git diff --cached      # staged only
git diff               # unstaged only
git log --oneline -5   # recent commit style reference
```

Also run `git stash list` to note any stashed work.

### Step 3 — Write the commit strategy

Decide: **one commit or multiple?**

**Bundle into one commit when:**
- All changes serve a single purpose (one bug fix, one feature, one refactor)
- Files are tightly coupled (e.g. implementation + its test)
- The diff is small and coherent

**Split into multiple commits when:**
- Changes touch unrelated concerns (e.g. a bug fix mixed with a refactor)
- Some changes are in a different domain (e.g. CI config + application logic)
- The conventional commit type differs across groups (feat vs fix vs chore)

Group the files and draft a message per commit.

### Step 4 — Write conventional commit messages

**If the `caveman-commit` skill is listed in the available skills**, invoke it via the Skill tool for each commit group instead of writing the message yourself:

```
Skill("caveman-commit", args="<brief description of what changed and why>")
```

Use the message it returns as-is.

**If `caveman-commit` is not available**, write the message manually using this format:

```
<type>(<optional scope>): <short summary in imperative mood>

<optional body: why, not what>

<optional footer: BREAKING CHANGE, closes #issue>
```

Types: `feat`, `fix`, `refactor`, `chore`, `docs`, `test`, `style`, `perf`, `ci`, `build`

Rules:
- Summary line ≤ 72 chars, lowercase after the colon, no trailing period
- Body explains *why*, not *what* — omit if obvious
- Mark breaking changes with `BREAKING CHANGE:` in the footer

### Step 5 — Present and ask for approval

Show the user:
- The proposed commit order
- Each commit's scope (which files)
- The full commit message

Clearly indicate each section (Commit number, message, scope, files) with consistent headers and formatting

Then ask:
> "Shall I apply these commits? You can also adjust the messages or grouping before I proceed."

Do not commit until the user explicitly confirms.

### Step 6 — Apply commits

For each commit in order:

```bash
git add <files for this commit>
git commit -m "$(cat <<'EOF'
<message>
EOF
)"
```

Never use `--no-verify` or skip hooks. If a hook rejects a commit, report the failure and stop — do not retry with bypass flags.

## Edge cases

- **Nothing to commit**: report it and stop.
- **Untracked files only**: ask whether to include them; don't add untracked files automatically.
- **Merge conflicts present**: report them, do not attempt to commit.
- **Detached HEAD**: warn the user before committing.
- **Hook failure**: report the hook output, stop, ask the user how to proceed.
