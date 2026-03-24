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
    ├─ bd create / bd update --status in_progress
    │
    ├─ superpowers:brainstorming (design before code)
    │
    ├─ superpowers:writing-plans (decompose into 2-5 min tasks)
    │
    ├─ superpowers:using-git-worktrees (isolate, non-trivial work)
    │
    ├─ For each task:
    │   ├─ TDD: RED → verify fail → GREEN → verify pass → REFACTOR
    │   ├─ Commit after each green
    │   └─ superpowers:requesting-code-review
    │
    ├─ superpowers:verification-before-completion
    │
    ├─ superpowers:finishing-a-development-branch
    │
    └─ bd close <id> --reason "Done"
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

- ALL work MUST be tracked: `bd create` or claim with `bd update <id> --status in_progress`
- `bd ready` → find available tasks (no blockers)
- `bd close <id> --reason "..."` → mark done
- Emergent work: `bd create "Issue" -t bug --deps discovered-from:<current-id>`
- Multi-terminal: each works on DIFFERENT issues (exclusive lock)

## When to Pull External Templates

Check `template-bridge:template-catalog` when task needs a specialist NOT in:

- Current project agents/skills
- Superpowers skills (14 workflow skills)
- Beads (task tracking)
- Other installed plugins (code-review, frontend-design, code-simplifier)

Install fresh: `npx claude-code-templates@latest --agent {category}/{name} --yes`

## Skill Invocation Priority

1. **Process skills FIRST**: brainstorming, debugging, verification
2. **Implementation skills SECOND**: TDD, code-review, frontend-design
3. **External templates THIRD**: only when no existing skill covers the need

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
