# Template Bridge Plugin

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code Plugin](https://img.shields.io/badge/Claude_Code-Plugin-blueviolet)](https://docs.anthropic.com/en/docs/claude-code)

Unified development workflow for Claude Code: **[Superpowers](https://github.com/obra/superpowers) + [Beads](https://github.com/steveyegge/beads) + [Templates](https://github.com/davila7/claude-code-templates)**.

Bridges three powerful Claude Code plugins into a single disciplined workflow with on-demand access to 413+ specialist agent templates.

## Prerequisites

| Plugin | Install | Purpose |
|--------|---------|---------|
| [Superpowers](https://github.com/obra/superpowers) | `/plugin install superpowers` | TDD, brainstorming, debugging, code review |
| [Beads](https://github.com/steveyegge/beads) | `/plugin install beads` | Task tracking across sessions |

Both are optional — the workflow skill adapts, but you get the full power with all three.

## What's Inside

| Component | Type | Purpose |
|-----------|------|---------|
| `unified-workflow` | Skill | How to work: beads task → brainstorm → plan → TDD → review → finish |
| `template-catalog` | Skill | 413+ specialist agents on-demand from [claude-code-templates](https://github.com/davila7/claude-code-templates) |
| `browse-templates` | Command | `/browse-templates` — manual template search |

## The Stack

```
┌─────────────────────────────────────────────────────┐
│                   YOUR TASK                          │
├──────────┬──────────────────┬───────────────────────┤
│  Beads   │   Superpowers    │   Template Bridge     │
│  WHAT    │   HOW            │   WHO                 │
│          │                  │                       │
│ bd create│ brainstorming    │ template-catalog      │
│ bd ready │ writing-plans    │ 413+ specialist agents│
│ bd close │ TDD (red-green)  │ on-demand install     │
│          │ debugging        │                       │
│          │ code-review      │                       │
│          │ verification     │                       │
└──────────┴──────────────────┴───────────────────────┘
```

## The Flow

Every task follows this sequence:

```
1. TASK        bd create -t feature "..."  or  bd update <id> --status in_progress
                │
2. BRAINSTORM   superpowers:brainstorming → design before code
                │
3. PLAN         superpowers:writing-plans → 2-5 min tasks
                │
4. ISOLATE      superpowers:using-git-worktrees (non-trivial work)
                │
5. IMPLEMENT    For each task:
                ├─ RED:    write ONE failing test
                ├─ VERIFY: confirm it fails for the right reason
                ├─ GREEN:  minimal code to pass
                ├─ VERIFY: all tests pass
                ├─ REFACTOR: clean up
                └─ COMMIT
                │
6. REVIEW       superpowers:requesting-code-review
                │
7. VERIFY       superpowers:verification-before-completion
                │
8. FINISH       superpowers:finishing-a-development-branch
                │
9. CLOSE        bd close <id> --reason "Done"
```

## TDD: Red-Green-Refactor

No production code without a failing test first. Ever.

```
RED ──────► VERIFY RED ──────► GREEN ──────► VERIFY GREEN ──────► REFACTOR ──────► COMMIT
write test   fails for        minimal        all tests           clean up          after each
             right reason     code           pass                                 green
```

## Debugging

Always invoke `superpowers:systematic-debugging` BEFORE any fix:

1. **Root cause** — read errors, reproduce, check recent changes
2. **Pattern analysis** — find working examples, compare
3. **Hypothesis** — single-variable test
4. **Fix** — with failing test first
5. **Stop** after 3 failed attempts → question architecture

## Verification Gate

Before claiming anything done:

1. Identify the proving command
2. Run it fresh
3. Read complete output + exit code
4. Verify it supports the claim
5. Only then claim done

> "Evidence before claims, always."

## External Templates

When a task needs a specialist not in current skills:

```bash
# Direct install (always fresh from repo)
npx claude-code-templates@latest --agent security/security-auditor --yes

# Interactive browser
npx claude-code-templates@latest
```

### Categories (26)

| Category | Top Agents |
|----------|------------|
| ai-specialists | prompt-engineer, llm-architect, model-evaluator |
| api-graphql | api-architect, graphql-specialist |
| blockchain-web3 | smart-contract-auditor, blockchain-developer |
| data-ai | data-engineer, ml-engineer, data-scientist |
| database | postgres-specialist, redis-expert |
| devops-infrastructure | kubernetes-specialist, terraform-expert, cloud-architect |
| development-team | backend-architect, frontend-developer, mobile-developer |
| development-tools | code-optimizer, dependency-manager |
| game-development | unity-developer, godot-specialist |
| mcp-dev-team | mcp-server-architect, mcp-developer |
| modernization | legacy-modernizer, architecture-migrator |
| performance-testing | performance-engineer, web-vitals-optimizer |
| programming-languages | rust-expert, go-specialist, python-architect |
| security | security-auditor, penetration-tester |
| ui-analysis | screenshot-analyzer, accessibility-auditor |
| web-tools | seo-analyzer, accessibility-specialist |

[Full list: 26 categories, 413+ agents](https://github.com/davila7/claude-code-templates)

## Skill Priority

1. **Process** — brainstorming, debugging, verification (always first)
2. **Implementation** — TDD, code-review, frontend-design (during work)
3. **Templates** — only when no existing skill covers the need

## Anti-Patterns

- Skip brainstorming → "just code it"
- Write code before failing test
- Claim "fixed" without running verification
- Work without beads task
- Multiple fixes simultaneously
- Trust subagent reports without own verification
- Qualifier language ("should work", "probably fixed")

## Installation

### From GitHub (recommended)

```bash
claude plugin marketplace add github:maslennikov-ig/template-bridge
claude plugin install template-bridge@template-bridge-marketplace
```

### From local clone

```bash
git clone https://github.com/maslennikov-ig/template-bridge.git
claude plugin marketplace add /path/to/template-bridge
claude plugin install template-bridge@template-bridge-marketplace
```

### Three layers of protection

The plugin uses three layers to ensure Claude **never forgets** the workflow, even in a fresh context:

| Layer | Mechanism | How it works |
|-------|-----------|-------------|
| **1. CLAUDE.md** | System prompt (highest priority) | Rules loaded before anything else, every session |
| **2. SessionStart hook** | Echo reminder after `bd prime` | Explicit reminder in context on session start |
| **3. Skill description** | Plugin skill listing | `unified-workflow` trigger visible in skill list |

### Layer 1: Copy CLAUDE.md to global config

The repo includes a ready-to-use [`CLAUDE.md`](CLAUDE.md). Copy its contents to your global config:

```bash
# Append to your global CLAUDE.md (create if doesn't exist)
cat /path/to/template-bridge/CLAUDE.md >> ~/.claude/CLAUDE.md
```

Or copy manually — the file is short (the full 9-step workflow + 4 rules).

### Layer 2 (optional): SessionStart hook

Add to `~/.claude/settings.json` for an extra reminder at every session start:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'WORKFLOW REMINDER: Invoke template-bridge:unified-workflow before any task. Flow: bd create → brainstorm → plan → TDD → review → verify → finish → bd close'"
          }
        ]
      }
    ]
  }
}
```

### Layer 3: Automatic

The skill description is always visible in Claude's skill listing — no setup needed.

## Updating

```bash
claude plugin install template-bridge@template-bridge-marketplace
```

Plugin re-reads from the source on reinstall.

## Contributing

1. Fork this repo
2. Edit skills or add new ones
3. Submit a PR

Plugin structure follows the [official Claude Code plugin spec](https://github.com/anthropics/claude-code).

## Credits

- [Superpowers](https://github.com/obra/superpowers) by Jesse Vincent — the TDD/brainstorming/debugging methodology
- [Beads](https://github.com/steveyegge/beads) by Steve Yegge — task tracking for AI workflows
- [claude-code-templates](https://github.com/davila7/claude-code-templates) by Daniel Avila — 413+ specialist agent templates

## License

[MIT](LICENSE)
