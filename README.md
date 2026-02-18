# CLAUDE.md Template

A reusable `CLAUDE.md` template for AI-assisted development projects using [Claude Code](https://claude.ai/claude-code).

## What is CLAUDE.md?

`CLAUDE.md` is a configuration file that lives in your project root. Claude Code reads it automatically to understand your project's conventions, preferences, and workflow rules. Think of it as a persistent set of instructions that shapes how Claude collaborates with you.

## What's in the template

| Section | Purpose |
|---------|---------|
| **Core Principles** | Simplicity, engineering preferences, code hygiene, respect for existing code |
| **Workflow Guidelines** | Architect-then-execute, iterative refinement, multi-concern sessions, communication rules |
| **Code Review Process** | Structured 4-stage review (architecture, code quality, tests, performance) with numbered issues and lettered options |
| **Bash Guidelines** | Avoiding output buffering issues and pipe pitfalls |
| **Code Quality Standards** | Error handling, testing (TDD), security |
| **Git Workflow** | Commit conventions, branching strategy |
| **Stack-Specific Guidelines** | Vercel, Supabase, TypeScript defaults |
| **Anti-Patterns** | Code bloat, over-documentation, scope creep |
| **Leverage Patterns** | Declarative over imperative, TDD, tool integration |

## Usage

1. Copy `CLAUDE.md` into your project root:

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/ah2484/claude-md-template/main/CLAUDE.md
```

2. Customize the sections to match your project:
   - Update **Stack-Specific Guidelines** for your tech stack
   - Update **Project Structure** to reflect your directory layout
   - Update **Environment Variables** with your required vars
   - Update **Quick Reference** with your project's commands

3. Commit it to your repo. Claude Code will pick it up automatically.

## Customization tips

- **Remove what you don't need.** If you're not using Supabase, delete that section.
- **Add project-specific rules.** Got a naming convention or a pattern you always follow? Add it.
- **Keep it concise.** Claude reads the entire file every session — shorter is better.
- **The Code Review Process section** is designed for plan mode. If you don't use structured code reviews, you can remove it.

## License

MIT
