@AGENTS.md

# This Fork — Read Before Doing Anything Here

This file is fork-only. Upstream deleted its `CLAUDE.md`; keep this one on every merge.

## What this repo is

This is Lior's personal fork of `obra/superpowers`. It has one job: it is the
Superpowers plugin that Claude Code uses on this Windows machine.

- `~/.claude` has a directory marketplace named `claude-superpowers` that points
  at `D:\Git\claude-superpowers`. Claude Code reads the plugin live from this
  folder (`claude plugin list` shows "Read from"), so every edit here takes
  effect immediately, with no reinstall.
- It is for Windows only, on this computer only. It is not published, and no
  other harness (Codex, OpenCode, and so on) uses it.

## What the fork changes

The fork is upstream plus these five customizations. Keep all five on every
merge. `README.md` describes each one in detail.

1. **No worktrees.** `using-git-worktrees` is deleted and no skill sends you to it.
2. **Git question restored.** `executing-plans` and `subagent-driven-development`
   ask once about branch and commit cadence.
3. **Doc paths.** Plans go to `docs/plans/`, specs to `docs/specs/` (see Fork
   Maintenance in `AGENTS.md`).
4. **Slim session start.** `skills/using-superpowers/SKILL.md` is the short bootstrap.
5. **Vitest wording** in `skills/test-driven-development/SKILL.md`.

## Updating from upstream

When Lior asks whether the fork needs an update, or asks to update it, this is
the whole job:

1. `git fetch upstream`, then compare `master` with `upstream/main`.
2. Merge `upstream/main` into `master` with `--no-commit`. Keep the five
   customizations; take upstream for everything else.
3. Run the Fork Maintenance checks in `AGENTS.md`.
4. Bump the version strings in `README.md` and `.claude-plugin/marketplace.json`.
5. Report the result. Commit only when Lior says so.

## Do not

- Do not fix, port, or chase failing tests for other harnesses or for upstream's
  release tooling (`tests/codex*`, `tests/opencode`, `tests/version-bump`, and
  similar). Many of them fail on Windows because they assume Linux tools.
  Mention them in one line at most.
- Do not install tools, download binaries, or patch upstream scripts to make
  tests pass.
- Do not use WSL for anything in this repo.
- Do not add changes beyond the five customizations unless Lior asks for them.
