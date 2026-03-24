## Workflow: Superpowers + Beads + Templates

Before starting ANY task, invoke `template-bridge:unified-workflow` skill to load the full workflow.

### Quick Reference (do NOT skip steps)

1. **Epic** — `bd create -t epic "Goal"` (container for intent + context)
2. **Brainstorm** — `superpowers:brainstorming` (design before code)
3. **Plan** — `superpowers:writing-plans` (2-5 min tasks)
4. **Sub-tasks** — `bd create` for each + `bd dep add` (parent-child, blocks)
5. **Isolate** — `superpowers:using-git-worktrees` (non-trivial work)
6. **Implement** — `bd ready` → pick → `bd update --claim` → TDD (RED → GREEN → REFACTOR)
7. **Review** — `superpowers:requesting-code-review`
8. **Verify** — `superpowers:verification-before-completion` (evidence before claims)
9. **Finish** — `superpowers:finishing-a-development-branch`
10. **Close** — `bd close <epic-id> --reason "Done"`

### Rules

- No production code without a failing test first
- No completion claims without running verification commands
- No work without a beads task
- Check `template-bridge:template-catalog` when a specialist agent is needed
- Side quests: `bd create -t bug` + `bd dep add new current --type discovered-from`
