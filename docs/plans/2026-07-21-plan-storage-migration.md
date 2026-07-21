# Plan Storage Migration Implementation Plan

> **For agentic workers:** Work in the current branch. Do not create a worktree or commit unless the user explicitly requests it.

**Goal:** Make `docs/plans/` the sole plan location and remove the legacy nested documentation namespace.

**Architecture:** Move existing dated records without changing their content except for exact internal path references. Update the planning skill, dependent skills, prompts, and test fixtures to use `docs/plans/`; keep specs independently in `docs/specs/` so their category remains clear.

**Tech Stack:** Git, Markdown, Shell test fixtures, PowerShell verification.

## Global Constraints

- Plan files are always written to `docs/plans/`.
- Specification files are stored in `docs/specs/`.
- Do not rewrite snapshot or screenshot directories; none exist in the current tree.
- Preserve document history through `git mv` and make only exact path substitutions.

---

### Task 1: Relocate the documentation trees

**Files:**
- Move: legacy plan records to `docs/plans/`
- Move: legacy specification records to `docs/specs/`

- [x] **Step 1: Verify the only legacy documentation root contains the plan and specification record trees.**

Run: `Get-ChildItem -Recurse -Force -Directory | Where-Object { $_.Name -ieq 'superpowers' }`

Expected: Only one legacy documentation root is returned.

- [x] **Step 2: Move both subtrees with Git.**

Run: `$legacyRoot = Join-Path docs superpowers; Get-ChildItem -File "$legacyRoot/plans" | ForEach-Object { git mv -- $_.FullName docs/plans/ }; git mv -- "$legacyRoot/specs" docs/specs`

Expected: The historical plan and specification files are tracked as renames, and the empty legacy directory disappears.

### Task 2: Normalize every reference

**Files:**
- Modify: `skills/writing-plans/SKILL.md`
- Modify: dependent skills, prompts, test fixtures, release notes, and moved documentation containing either old path

- [x] **Step 1: Replace the legacy plan and specification path prefixes across tracked text.**

Replace the legacy plan prefix with `docs/plans/` and the legacy specification prefix with `docs/specs/`.

- [x] **Step 2: Verify no old path remains.**

Run: `rg -n ('docs/' + 'superpowers') --glob '!node_modules/**' --glob '!evals/**' .`

Expected: No matches.

### Task 3: Verify behavior and layout

**Files:**
- Verify: `skills/writing-plans/SKILL.md`
- Verify: `tests/claude-code/`, `tests/explicit-skill-requests/`, and `docs/`

- [x] **Step 1: Confirm the planning skill’s save instruction names `docs/plans/`.**

Run: `rg -n "Save every implementation plan to:|Plan complete and saved" skills/writing-plans/SKILL.md`

Expected: Both instructions use `docs/plans/`.

- [x] **Step 2: Confirm the old directory and old references are absent.**

Run: `$legacyRoot = Join-Path docs superpowers; Test-Path $legacyRoot; rg -n ('docs/' + 'superpowers') .`

Expected: `False` followed by no search results.

- [x] **Step 3: Inspect the migration diff for whitespace errors and unrelated edits.**

Run: `git diff --check; git diff --stat`

Expected: No whitespace errors; only the documentation relocation and exact path updates appear.
