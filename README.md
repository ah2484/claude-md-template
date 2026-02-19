# CLAUDE.md / AGENTS.md Templates

Reusable templates for AI-assisted development projects using [Claude Code](https://claude.ai/claude-code) and [OpenAI Codex](https://openai.com/index/introducing-codex/).

## What are these files?

| File | Agent | Description |
|------|-------|-------------|
| `CLAUDE.md` | Claude Code | Read automatically by Claude Code to understand your project's conventions, preferences, and workflow rules. |
| `AGENTS.md` | OpenAI Codex | Read automatically by Codex before every task to guide how it navigates your codebase, runs tests, and follows your standards. |

Both files serve the same purpose — a persistent set of instructions that shapes how AI agents collaborate with you. They share the same core principles but are adapted for each agent's workflow.

## Key differences

| | CLAUDE.md | AGENTS.md |
|---|-----------|-----------|
| **Interaction model** | Interactive (real-time conversation) | Async (task-based, creates PRs) |
| **Clarification** | Can ask questions mid-task | Must work from instructions alone |
| **Code review** | Interactive staged review with numbered issues | Checklist-based, comments left on PR |
| **Workflow** | Plan mode, iterative refinement with feedback | Self-contained task execution |
| **Integrations** | Skills.sh, MCPs | N/A |

## What's in the templates

| Section | Purpose |
|---------|---------|
| **Core Principles** | Simplicity, engineering preferences, code hygiene, respect for existing code |
| **Workflow / Working Agreements** | Task execution, PR quality, communication rules |
| **Code Review** | Architecture, code quality, tests, performance |
| **Shell / Bash Guidelines** | Avoiding output buffering issues and pipe pitfalls |
| **Code Quality Standards** | Error handling, testing (TDD), security |
| **Git Workflow** | Commit conventions, branching strategy |
| **Stack-Specific Guidelines** | Vercel (including env var trailing newline gotcha), Supabase, TypeScript defaults |
| **Anti-Patterns** | Code bloat, over-documentation, scope creep |

## Usage

### Claude Code

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/ah2484/claude-md-template/main/CLAUDE.md
```

### OpenAI Codex

```bash
curl -o AGENTS.md https://raw.githubusercontent.com/ah2484/claude-md-template/main/AGENTS.md
```

### Both (recommended if you use both agents)

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/ah2484/claude-md-template/main/CLAUDE.md
curl -o AGENTS.md https://raw.githubusercontent.com/ah2484/claude-md-template/main/AGENTS.md
```

Then customize the sections to match your project:
- Update **Stack-Specific Guidelines** for your tech stack
- Update **Project Structure** to reflect your directory layout
- Update **Environment Variables** with your required vars
- Update **Quick Reference** with your project's commands

Commit them to your repo. Each agent will pick up its respective file automatically.

## Customization tips

- **Remove what you don't need.** If you're not using Supabase, delete that section.
- **Add project-specific rules.** Got a naming convention or a pattern you always follow? Add it.
- **Keep it concise.** Both agents read the entire file every session — shorter is better.
- **CLAUDE.md's Code Review Process section** is designed for interactive plan mode. If you don't use structured code reviews, you can remove it.
- **AGENTS.md's Working Agreements section** is tuned for async PR workflows. Adjust to match how you review PRs.

## License

MIT
