---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints
---

# Executing Plans

## Overview

Load plan, review critically, execute all tasks, report when complete.

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

**Note:** Tell your human partner that Superpowers works much better with access to subagents. The quality of its work will be significantly higher if run on a platform with subagent support (Claude Code, Codex CLI, Codex App, and Copilot CLI all qualify; see the per-platform tool refs in `../using-superpowers/references/`). If subagents are available, use superpowers:subagent-driven-development instead of this skill.

## The Process

### Step 1: Load and Review Plan
1. Read plan file
2. Review critically - identify any questions or concerns about the plan
3. If concerns: Raise them with your human partner before starting
4. If no concerns: Create todos for the plan items and proceed

### Step 2: Confirm Git Workflow

Work happens in place on the current branch — there is no worktree to set up —
but the branch and commit approach still need the user's consent before any
code is written.

Report the current branch (`git branch --show-current`), then ask exactly this:

> You're on branch `<current-branch>`. How should I handle git for this work?
>
> 1. Create a new branch, then commit after each task
> 2. Stay on `<current-branch>`, commit after each task
> 3. Stay on `<current-branch>`, one commit at the end
> 4. Don't commit — I'll handle commits myself
>
> Which option?

- Honor any git preference the user already stated in their instructions — do
  not re-ask.
- **Never** start implementation on `main`/`master` without explicit consent.
  If the current branch is `main`/`master` and the user did not pick option 1,
  confirm before proceeding.
- Remember the chosen cadence — Step 3 commits according to it.

### Step 3: Execute Tasks

For each task:
1. Mark as in_progress
2. Follow each step exactly (plan has bite-sized steps)
3. Run verifications as specified
4. Commit per the cadence chosen in Step 2 (per-task cadence commits the task
   now; end-of-run and manual cadences do not commit here)
5. Mark as completed

### Step 4: Complete Development

After all tasks complete and verified:
- If Step 2 chose "one commit at the end," create that single commit now.
- Announce: "I'm using the finishing-a-development-branch skill to complete this work."
- **REQUIRED SUB-SKILL:** Use superpowers:finishing-a-development-branch
- Follow that skill to verify tests, present options, execute choice

## When to Stop and Ask for Help

**STOP executing immediately when:**
- Hit a blocker (missing dependency, test fails, instruction unclear)
- Plan has critical gaps preventing starting
- You don't understand an instruction
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## When to Revisit Earlier Steps

**Return to Review (Step 1) when:**
- Partner updates the plan based on your feedback
- Fundamental approach needs rethinking

**Don't force through blockers** - stop and ask.

## Remember
- Review plan critically first
- Follow plan steps exactly
- Don't skip verifications
- Reference skills when plan says to
- Stop when blocked, don't guess
- Never start implementation on main/master branch without explicit user consent

## Integration

**Work in place on the current branch** — no separate isolated workspace is created.

**Required workflow skills:**
- **superpowers:writing-plans** - Creates the plan this skill executes
- **superpowers:finishing-a-development-branch** - Complete development after all tasks
