# claude-superpowers (Lior's fork)

This is Lior's personal fork of the [Superpowers](https://github.com/obra/superpowers) Claude Code plugin, seeded from version **6.1.0** as distributed via the official marketplace `anthropics/claude-plugins-official` at commit `a98c5dfc`.

It keeps everything about upstream Superpowers except for the customizations below. The plugin's internal name is unchanged (`superpowers`), so skill invocations remain `superpowers:<skill>`.

## Why this fork exists

Upstream Superpowers' workflow skills isolate feature work in a git worktree (`using-git-worktrees`, invoked by `executing-plans`, `subagent-driven-development`, `finishing-a-development-branch`, `writing-plans`). Lior's workflow always works in place on the current branch and never uses git worktrees. Upstream's `SessionStart` hook also injects the full `using-superpowers/SKILL.md` (~650 tokens) into every session; a trimmed version keeps the essential rules without the bulk.

## Customizations vs. upstream 6.1.0

1. **No worktrees.** `skills/using-git-worktrees/` is deleted entirely. Every skill that required or referenced it (`executing-plans`, `subagent-driven-development`, `finishing-a-development-branch`, `writing-plans`, plus incidental mentions in `requesting-code-review` and `subagent-driven-development/scripts/task-brief`) now describes working in place on the current branch instead. `finishing-a-development-branch` had the most surgery: its environment-detection and cleanup steps assumed a possible worktree checkout; that logic is removed since the fork only ever runs in the normal (non-worktree) repo state.
2. **Slim SessionStart injection.** `skills/using-superpowers/SKILL.md` is trimmed from ~650 tokens to ~200 tokens (~160-word body). It keeps: the invoke-before-acting rule, the skill-priority rule (process skills before implementation skills), and user-instructions-precedence (CLAUDE.md/AGENTS.md/direct requests override skills). It drops the Red Flags table and the Platform Adaptation section. The `SUBAGENT-STOP` block is kept as-is. The `references/{codex,pi,antigravity}-tools.md` files under `using-superpowers/` were **not** deleted even though nothing in the trimmed `SKILL.md` links to them anymore — upstream's own test suites (`tests/pi/test-pi-extension.mjs`, `tests/antigravity/test-antigravity-tools.sh`) assert their existence/content directly, so removing them would just create a different kind of dangling reference.
3. **TypeScript/vitest wording.** `test-driven-development/SKILL.md` swaps its one Jest-flavored mock example (`jest.fn()` → `vi.fn()`) and its `npm test <file>` command examples for `npm run test:run <file>`, matching this stack's actual `package.json` scripts. `verification-before-completion/SKILL.md` had no language-specific wording to begin with, so it's untouched.

Everything else — brainstorming, systematic-debugging, TDD, code review, plan writing/execution, subagent-driven development, the hooks/session-start mechanism, the multi-platform adapters (Codex/Cursor/Kimi/OpenCode/Pi/Antigravity) — is unmodified upstream content.

## Diffing against a future upstream version

The first commit in this repo (`seed: superpowers 6.1.0 from claude-plugins-official @a98c5dfc`) is a byte-for-byte copy of the upstream 6.1.0 tree with no customizations applied. To see everything Lior's fork changed relative to that pristine baseline:

```bash
git diff 42743dc HEAD
```

To pull in a newer upstream release, copy the new version's tree over a fresh checkout of the seed commit, commit that as a new pristine baseline, then re-apply (or manually re-diff) the customizations above against the new baseline — the customization commit(s) on top of the *old* seed serve as the checklist of what to re-apply.

## Provenance

- Upstream project: https://github.com/obra/superpowers
- Distributed via marketplace: `anthropics/claude-plugins-official`
- Marketplace commit: `a98c5dfc`
- Seeded version: `6.1.0`
- Source cache path at seed time: `~/.claude/plugins/cache/claude-plugins-official/superpowers/6.1.0/`
