## Workflow: Superpowers + Beads + Templates

Before starting ANY task, invoke `template-bridge:unified-workflow` skill to load the full workflow.

### Quick Reference (do NOT skip steps)

1. **Track** — `bd create` or `bd update <id> --status in_progress`
2. **Brainstorm** — `superpowers:brainstorming` (design before code)
3. **Plan** — `superpowers:writing-plans` (2-5 min tasks)
4. **Isolate** — `superpowers:using-git-worktrees` (non-trivial work)
5. **Implement** — `superpowers:test-driven-development` (RED → GREEN → REFACTOR)
6. **Review** — `superpowers:requesting-code-review`
7. **Verify** — `superpowers:verification-before-completion` (evidence before claims)
8. **Finish** — `superpowers:finishing-a-development-branch`
9. **Close** — `bd close <id> --reason "Done"`

### Rules

- No production code without a failing test first
- No completion claims without running verification commands
- No work without a beads task
- Check `template-bridge:template-catalog` when a specialist agent is needed
