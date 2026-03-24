---
name: unified-workflow
version: 1.0.0
description: >
  Unified development methodology: Superpowers + Beads + Templates.
  TRIGGER: At session start, before any task, or when workflow is unclear.
  Defines the mandatory flow: beads task → brainstorm → plan → TDD → review → finish.
---

# Unified Development Workflow

## The Stack

| System | Role | Type |
|--------|------|------|
| **Superpowers** | Development methodology (HOW to work) | Plugin, rigid skills |
| **Beads** | Task tracking (WHAT to work on) | Plugin, `bd` CLI |
| **Template Bridge** | Specialist agents (WHO helps) | On-demand from claude-code-templates |

## The Mandatory Flow

```
Task arrives
    │
    ├─ 1. EPIC: bd create -t epic "High-level goal"
    │       (container — tracks intent, holds lock, preserves context)
    │
    ├─ 2. BRAINSTORM: superpowers:brainstorming (design before code)
    │
    ├─ 3. PLAN: superpowers:writing-plans (decompose into 2-5 min tasks)
    │
    ├─ 4. SUB-TASKS: Create concrete tasks from the plan:
    │       bd create "Sub-task 1" -t task
    │       bd dep add sub-task-1 epic-id --type parent-child
    │       bd dep add sub-task-2 sub-task-1  (if sequential — blocks)
    │
    ├─ 5. ISOLATE: superpowers:using-git-worktrees (non-trivial work)
    │
    ├─ 6. IMPLEMENT: bd ready → pick from Ready Front → bd update <id> --claim
    │       For each sub-task:
    │       ├─ TDD: RED → verify fail → GREEN → verify pass → REFACTOR
    │       ├─ Commit after each green
    │       ├─ superpowers:requesting-code-review
    │       └─ bd close <sub-id> --reason "Done"
    │           (closing unblocks next Ready Front automatically)
    │
    ├─ 7. VERIFY: superpowers:verification-before-completion
    │
    ├─ 8. FINISH: superpowers:finishing-a-development-branch
    │
    └─ 9. CLOSE: bd close <epic-id> --reason "Done"
```

### Task Decomposition with Beads Dependencies

**Four dependency types** (only `blocks` affects `bd ready`):

| Type | Effect on `bd ready` | Use when |
|------|---------------------|----------|
| **blocks** | YES — blocked task hidden from `bd ready` | Sequential work, technical prerequisites |
| **parent-child** | No | Epic → sub-task structure |
| **related** | No | Connected but independent work |
| **discovered-from** | No | Side-quest found during implementation |

**Direction rule**: Think "X needs Y" → `bd dep add X Y`

**Ready Fronts**: As sub-tasks close, blocked work automatically becomes ready.
Use `bd ready` to always see what can be worked on NOW.

**Side quests during implementation**:
```
bd create "Found: missing input validation" -t bug
bd dep add new-id current-id --type discovered-from
```

## TDD: Red-Green-Refactor (RIGID — no exceptions)

1. **RED**: Write ONE failing test for desired behavior
2. **VERIFY RED**: Run it. Must fail for the RIGHT reason
3. **GREEN**: Write minimal code to pass
4. **VERIFY GREEN**: All tests pass
5. **REFACTOR**: Clean up, tests still pass
6. **COMMIT** after each green cycle

**No production code without a failing test first. Ever.**

## Debugging (RIGID)

Invoke `superpowers:systematic-debugging` BEFORE any fix attempt:

1. Root cause investigation (read errors, reproduce, check recent changes)
2. Pattern analysis (find working examples, compare)
3. Hypothesis + single-variable test
4. Fix with failing test first
5. Stop after 3 failed attempts → question architecture

## Verification Gate (RIGID)

Before claiming ANYTHING done, invoke `superpowers:verification-before-completion`:

1. Identify the proving command
2. Run it fresh (not cached)
3. Read complete output + exit code
4. Verify output supports claim
5. Only then claim done

**"Evidence before claims, always."**

## Beads Task Tracking

- ALL work MUST be tracked — start with epic, decompose after plan
- `bd ready` → find available tasks from current Ready Front
- `bd update <id> --claim` → atomic start (marks in_progress + lock)
- `bd close <id> --reason "..."` → mark done, auto-unblocks dependents
- Side quests: `bd create "Issue" -t bug` + `bd dep add new-id current-id --type discovered-from`
- Multi-terminal: each works on DIFFERENT issues (exclusive lock)
- Session recovery: `bd list --status in_progress` → `bd show <id>` → read notes

## When to Pull External Templates

Check `template-bridge:template-catalog` when task needs a specialist NOT in:

- Current project agents/skills
- Superpowers skills (14 workflow skills)
- Beads (task tracking)
- Other installed plugins (code-review, frontend-design, code-simplifier)

Install fresh: `npx claude-code-templates@latest --agent {category}/{name} --yes`

## Context7: Fresh Docs (MANDATORY)

Before implementing with ANY external library, framework, or SDK:

```
mcp__context7__resolve-library-id → mcp__context7__query-docs
```

**When**: Next.js, Supabase, React, Tailwind, Zod, BullMQ, Qdrant, LangChain — any library.
**Who**: Main agent, subagents, and skills — everyone queries Context7.
**Why**: Claude's training data is stale. APIs change. Context7 gives current docs.

**Never write implementation code based on memory alone. Always verify with Context7 first.**

## Skill Invocation Priority

1. **Context7 FIRST**: query fresh docs for any library/framework being used
2. **Process skills SECOND**: brainstorming, debugging, verification
3. **Implementation skills THIRD**: TDD, code-review, frontend-design
4. **External templates FOURTH**: only when no existing skill covers the need

## Key Superpowers Skills Reference

| Skill | When | Type |
|-------|------|------|
| `brainstorming` | Before ANY creative/feature work | Rigid |
| `writing-plans` | After brainstorm approval | Rigid |
| `test-driven-development` | During ALL implementation | Rigid |
| `systematic-debugging` | Any bug/test failure | Rigid |
| `verification-before-completion` | Before claiming done | Rigid |
| `using-git-worktrees` | Non-trivial feature work | Flexible |
| `requesting-code-review` | After each task | Flexible |
| `receiving-code-review` | When getting feedback | Rigid |
| `subagent-driven-development` | Executing plans, independent tasks | Flexible |
| `executing-plans` | Following pre-written plans | Flexible |
| `finishing-a-development-branch` | Tests pass, ready to merge | Rigid |
| `dispatching-parallel-agents` | 2+ independent problems | Flexible |

## Anti-Patterns (NEVER)

- Skip brainstorming → "just code it"
- Write code before failing test
- Claim "fixed" without verification command output
- Work without beads task
- Multiple fixes simultaneously
- Trust subagent reports without own verification
- Qualifier language ("should work", "probably fixed")
- Implement with library without querying Context7 first
