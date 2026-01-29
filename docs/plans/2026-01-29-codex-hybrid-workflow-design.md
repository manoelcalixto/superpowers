# Codex Hybrid Workflow Design (Superpowers)

Date: 2026-01-29

## Summary

This design makes the Superpowers workflow more compatible with OpenAI Codex by adopting a hybrid multi-agent strategy: subagents are used only for review and diagnostics, while implementation remains in the main agent. The goal is to leverage Codex's emerging collaboration tools without exposing users to the instability and scope drift observed in subagent execution (worktree changes, premature finishing, or re-running workflow steps).

## Goals

- Make Codex guidance first-class in docs, bootstrap, and AGENTS.md snippets.
- Remove Claude-centric tool assumptions from shared skills or provide explicit Codex fallbacks.
- Establish strict guardrails for Codex subagents so they cannot exceed scope.
- Preserve compatibility with Claude Code/OpenCode by keeping shared skill structure intact.

## Non-Goals

- Implementing Codex tooling changes (this repo only adapts Superpowers content).
- Redesigning the entire skill system or workflow order.
- Enforcing subagent behavior via runtime controls (only prompt-level guardrails).

## Key Decisions

1. **Hybrid Mode Default for Codex**
   Subagents are allowed only for review/diagnostic tasks. The main agent performs all edits, tests, and commits.

2. **Strict Subagent Guardrails**
   Subagent prompts explicitly forbid:
   - Creating/removing worktrees
   - Finishing a development branch
   - Spawning other agents
   - Modifying global config or AGENTS.md

3. **Codex-first docs and bootstrap**
   Codex docs explicitly mention the hybrid mode and how to opt in to collaboration tools when available.

4. **Shared skills remain shared**
   Shared skills add conditional language for Codex behavior rather than forking separate variants.

## Workflow Overview

### Main Agent

- Follows standard Superpowers workflows (brainstorm -> plan -> execute -> finish).
- Executes tasks directly in the repo and runs verification steps.
- Uses subagents only for:
  - Spec compliance review
  - Code quality review
  - Focused diagnostics

### Subagents (Codex)

- Operate as reviewers or diagnosticians.
- Provide findings only; do not modify repo state.
- Return short, actionable reports.

## Data Flow

1. Main agent identifies review/diagnostic need.
2. Main agent spawns subagent with constrained prompt template.
3. Subagent returns findings.
4. Main agent applies fixes or proceeds.

## Error Handling

- If subagent exceeds scope, main agent stops delegation and continues manually.
- If subagent output is incomplete, main agent requests clarification or re-runs review manually.
- If subagents are not available, main agent uses local review templates.

## Testing / Verification

- No runtime tests required for documentation/skill changes.
- Validate via:
  - Bootstrap output readability
  - Skills documentation consistency
  - Codex docs alignment with current collaboration capabilities

## Planned Changes (High-Level)

1. **Docs/Bootstrap**
   - Update `.codex/superpowers-bootstrap.md` and `.codex/INSTALL.md` to explain hybrid mode.
   - Update `docs/README.codex.md` to include collaboration guardrails and fallback behavior.

2. **Shared Skills**
   - Update `skills/subagent-driven-development/SKILL.md` to include Codex hybrid branch.
   - Update reviewer prompt templates to explicitly prohibit worktree/branch/agent actions.
   - Update `skills/requesting-code-review/SKILL.md` with Codex fallback if subagents unavailable.

3. **CLI Messaging**
   - Update `.codex/superpowers-codex` output to mention hybrid mode for Codex if feasible.

## Open Questions

- Do we want to include a minimal Codex config snippet (agents.max_threads) in docs?
- Should subagent usage be globally opt-in for Codex, or default to off with a single toggle?

