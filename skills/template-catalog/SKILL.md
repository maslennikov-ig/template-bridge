---
name: template-catalog
version: 1.0.0
description: >
  Proactively suggest and install specialist agent templates from claude-code-templates
  (413+ agents, 26 categories). TRIGGER: When a task needs a specialist agent NOT
  available in current skills/agents. Examples: blockchain, game dev, data science,
  Kubernetes, database optimization, Rust/Go experts, video processing.
---

# Template Catalog — 413+ Specialist Agents

## How It Works

1. You see a task that would benefit from a specialist not in current agents
2. Suggest to user: "For this task there's a {name} template — install it?"
3. Install: `npx claude-code-templates@latest --agent {category}/{name} --yes`
4. Agent goes to `.claude/agents/{name}.md` in current project
5. Use the agent, optionally remove after

## IMPORTANT: Check existing skills FIRST

Before suggesting a template, verify it's not already covered by:

- Superpowers skills (brainstorming, TDD, debugging, code-review, etc.)
- Beads (task tracking)
- Project-specific agents (check .claude/agents/)
- Other plugins (frontend-design, code-simplifier, skill-creator)

## Available Categories

| Category | Count | Top Agents |
|----------|-------|------------|
| ai-specialists | 8 | prompt-engineer, llm-architect, model-evaluator |
| api-graphql | 8 | api-architect, graphql-specialist, rest-api-designer |
| blockchain-web3 | 3 | smart-contract-auditor, blockchain-developer, web3-architect |
| business-marketing | 6 | growth-strategist, seo-specialist, content-strategist |
| data-ai | 12 | data-engineer, ml-engineer, data-scientist, nlp-specialist |
| database | 8 | postgres-specialist, redis-expert, mongodb-architect |
| deep-research-team | 4 | research-analyst, fact-checker |
| development-team | 18 | backend-architect, frontend-developer, mobile-developer |
| development-tools | 34 | code-optimizer, dependency-manager, migration-specialist |
| devops-infrastructure | 15 | kubernetes-specialist, terraform-expert, cloud-architect |
| documentation | 6 | api-docs-writer, technical-writer |
| expert-advisors | 5 | domain-expert, consultant |
| ffmpeg-clip-team | 3 | video-editor, media-processor |
| finance | 4 | fintech-developer, payment-specialist |
| game-development | 5 | unity-developer, godot-specialist, game-designer |
| git | 6 | git-flow-manager, merge-specialist |
| mcp-dev-team | 5 | mcp-server-architect, mcp-developer, mcp-tester |
| modernization | 4 | legacy-modernizer, architecture-migrator |
| obsidian-ops-team | 3 | obsidian-plugin-dev, knowledge-manager |
| ocr-extraction-team | 3 | document-processor, ocr-specialist |
| performance-testing | 8 | performance-engineer, load-tester, web-vitals-optimizer |
| podcast-creator-team | 3 | podcast-producer, audio-engineer |
| programming-languages | 20+ | rust-expert, go-specialist, python-architect, java-expert |
| realtime | 4 | websocket-specialist, event-driven-architect |
| security | 12 | security-auditor, penetration-tester, compliance-specialist |
| ui-analysis | 5 | screenshot-analyzer, accessibility-auditor, ux-reviewer |
| web-tools | 8 | seo-analyzer, accessibility-specialist, web-scraper |

## Install Examples

```bash
# Security audit
npx claude-code-templates@latest --agent security/security-auditor --yes

# Kubernetes specialist
npx claude-code-templates@latest --agent devops-infrastructure/kubernetes-specialist --yes

# Rust expert
npx claude-code-templates@latest --agent programming-languages/rust-expert --yes

# Interactive browser (all categories)
npx claude-code-templates@latest
```

## Source

Repository: https://github.com/davila7/claude-code-templates
Always fetches latest version (no stale copies).
