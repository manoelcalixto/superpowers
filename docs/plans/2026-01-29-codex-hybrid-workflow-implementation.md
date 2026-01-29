# Codex Hybrid Workflow Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Make Superpowers more Codex-compatible by adopting a hybrid multi-agent workflow (subagents for review/diagnostics only) and updating docs, bootstrap, CLI messaging, and shared skills accordingly.

**Architecture:** Update Codex-specific docs/bootstraps to describe hybrid behavior, then adjust shared skills and subagent prompts to enforce strict guardrails and provide Codex fallbacks. Keep shared skill structure intact and avoid Claude-specific tool assumptions.

**Tech Stack:** Markdown docs, Node CLI script, Superpowers skills (SKILL.md), prompt templates.

### Task 1: Update Codex docs and bootstrap to describe hybrid mode

**Files:**
- Modify: `docs/README.codex.md`
- Modify: `.codex/INSTALL.md`
- Modify: `.codex/superpowers-bootstrap.md`

**Step 1: Add hybrid mode section to Codex docs**

Edit `docs/README.codex.md` to add a section describing:
- Hybrid default (subagents for review/diagnostics only)
- Guardrails (no worktrees, no finishing branch, no nested subagents)
- Fallback behavior when subagents are unavailable

**Step 2: Update Codex installation instructions**

Edit `.codex/INSTALL.md` to append a short “Hybrid Mode Guidance” block beneath the AGENTS.md snippet.

**Step 3: Update Codex bootstrap text**

Edit `.codex/superpowers-bootstrap.md` to include a short hybrid‑mode paragraph (after Tool Mapping) with the guardrails and fallback behavior.

**Step 4: Manual verification**

Open the three files and confirm the hybrid guidance appears exactly once and uses Codex terminology.

**Step 5: Commit**

```bash
git add docs/README.codex.md .codex/INSTALL.md .codex/superpowers-bootstrap.md
git commit -m "docs: add Codex hybrid multi-agent guidance"
```

### Task 2: Update Codex CLI messaging for hybrid mode

**Files:**
- Modify: `.codex/superpowers-codex`

**Step 1: Add hybrid guidance to bootstrap output**

Edit `runBootstrap()` to print a brief Codex hybrid mode note (1–2 lines) near the bootstrap instructions section.

**Step 2: Update help text**

In `runFindSkills()` or the final “Bootstrap Complete” footer, add a single line noting hybrid mode (subagents for review/diagnostics only).

**Step 3: Manual verification**

Run `~/.codex/superpowers/.codex/superpowers-codex bootstrap` and confirm the new note appears once and does not duplicate the bootstrap file content.

**Step 4: Commit**

```bash
git add .codex/superpowers-codex
git commit -m "codex: mention hybrid multi-agent mode in bootstrap"
```

### Task 3: Update shared skills for Codex hybrid behavior

**Files:**
- Modify: `skills/subagent-driven-development/SKILL.md`
- Modify: `skills/requesting-code-review/SKILL.md`
- Modify: `skills/dispatching-parallel-agents/SKILL.md`

**Step 1: Add Codex hybrid branch to subagent-driven development**

Edit `skills/subagent-driven-development/SKILL.md` to add a short “Codex Hybrid” section:
- In Codex, do not dispatch implementer subagents
- Use subagents only for review/diagnostics
- Implementation stays in main agent
- If subagents unavailable, do manual review

**Step 2: Add Codex fallback to requesting-code-review**

Edit `skills/requesting-code-review/SKILL.md` to clarify:
- If Codex subagents are unavailable, the main agent runs the review using the template checklist

**Step 3: Update dispatching-parallel-agents for Codex**

Edit `skills/dispatching-parallel-agents/SKILL.md` to add a short Codex note:
- Use `spawn_agent`/`wait` when available
- Keep subagent scope strict and no side effects

**Step 4: Manual verification**

Scan each skill file to ensure descriptions remain trigger-only (no workflow summary in frontmatter).

**Step 5: Commit**

```bash
git add skills/subagent-driven-development/SKILL.md \
  skills/requesting-code-review/SKILL.md \
  skills/dispatching-parallel-agents/SKILL.md
git commit -m "skills: add Codex hybrid guidance and fallbacks"
```

### Task 4: Add guardrails to subagent prompt templates

**Files:**
- Modify: `skills/subagent-driven-development/implementer-prompt.md`
- Modify: `skills/subagent-driven-development/spec-reviewer-prompt.md`
- Modify: `skills/subagent-driven-development/code-quality-reviewer-prompt.md`
- Modify: `skills/requesting-code-review/code-reviewer.md`

**Step 1: Add guardrail block to implementer prompt**

Insert a short “Guardrails” block that forbids:
- Creating/removing worktrees
- Finishing a development branch
- Spawning other agents
- Editing AGENTS.md or global config

**Step 2: Add guardrail block to reviewer prompts**

Add the same guardrails to spec reviewer and code quality reviewer prompts.

**Step 3: Add guardrail block to code-reviewer template**

Add a short “Scope” section stating reviewers should not modify repo state.

**Step 4: Manual verification**

Open all four prompt files and confirm guardrails are consistent and concise.

**Step 5: Commit**

```bash
git add skills/subagent-driven-development/implementer-prompt.md \
  skills/subagent-driven-development/spec-reviewer-prompt.md \
  skills/subagent-driven-development/code-quality-reviewer-prompt.md \
  skills/requesting-code-review/code-reviewer.md
git commit -m "skills: add subagent guardrails for Codex hybrid mode"
```

### Task 5: Update top-level README Codex section (brief)

**Files:**
- Modify: `README.md`

**Step 1: Add a short Codex hybrid mention**

In the Codex section, add 1–2 sentences noting the hybrid default and where to read more (`docs/README.codex.md`).

**Step 2: Manual verification**

Ensure the README change is short and does not duplicate full docs.

**Step 3: Commit**

```bash
git add README.md
git commit -m "docs: mention Codex hybrid workflow in README"
```

