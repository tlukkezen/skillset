---
name: feature-request
description: Implement a feature request on the current repository. Starts by discovering available context files (README, CONTEXT.md, CLAUDE.md, ADRs, etc.) before planning and implementing. Use when the user asks to add a feature, implement something new, or extend existing functionality in the codebase.
---

# Feature Request

## Workflow

1. **Discover context**
   - Check for `CONTEXT.md`, `README.md`, `CLAUDE.md`, `docs/`, `ADR/`, or similar files in `/workspace`
   - Read whichever exist — prioritise project-specific context over generic README prose
   - Check `/workspace/.claude/CLAUDE.md` and `CLAUDE.md` at repo root if present

2. **Understand the codebase**
   - List top-level directory structure to find source layout
   - Identify language, framework, and build tooling from config files (`package.json`, `pyproject.toml`, `Makefile`, `Dockerfile`, etc.)
   - Search for code related to the feature area before writing anything new

3. **Plan**
   - State what you intend to change and why, in one short paragraph
   - Identify affected files; flag any ambiguities before touching code
   - If the feature is large or the approach is non-obvious, confirm the plan with the user first

4. **Implement**
   - Make the minimal set of changes that satisfies the request
   - Follow the conventions already present in the codebase (naming, formatting, structure)
   - Do not add error handling, abstractions, or config flags beyond what the request requires

5. **Verify**
   - Run existing tests or build steps if available
   - Confirm the feature works as described

## Notes

- Never assume the project structure — always discover it first
- If a `CONTEXT.md` exists, treat its glossary and decisions as authoritative
- Write no comments unless the why is non-obvious
