# Project Guidelines for Claude

> A reusable CLAUDE.md template for AI-assisted development projects.

---

## Core Principles

### Simplicity Over Complexity
- **Avoid over-engineering.** Only make changes that are directly requested or clearly necessary.
- Do not add features, refactor code, or make "improvements" beyond what was asked.
- A bug fix doesn't need surrounding code cleaned up. A simple feature doesn't need extra configurability.
- Prefer 3 similar lines of code over a premature abstraction.
- The right amount of complexity is the minimum needed for the current task.

### Ask Before Assuming
- **Never make assumptions on my behalf.** If requirements are unclear, ask for clarification.
- Surface inconsistencies when you find them.
- Present tradeoffs when multiple valid approaches exist.
- Push back when a request seems problematic—don't be sycophantic.
- If something seems wrong or confusing, say so rather than proceeding silently.

### Code Hygiene
- **Clean up after yourself.** Remove dead code, unused imports, and obsolete comments.
- Do not leave `TODO` comments for things you could fix now.
- Do not modify code, comments, or files unrelated to the current task.
- Do not add docstrings, comments, or type annotations to code you didn't change.
- Only add comments where the logic isn't self-evident.

### Respect Existing Code
- **Read before writing.** Always read and understand existing code before modifying it.
- Follow existing patterns and conventions in the codebase.
- Don't rename variables, restructure files, or "improve" formatting as side effects.
- If existing code works, don't refactor it unless explicitly asked.

---

## Workflow Guidelines

### Plan Mode First
- For non-trivial changes, **start in plan mode** to discuss approach before implementation.
- Break complex tasks into clear, sequential steps.
- Identify potential risks or edge cases upfront.
- Get approval on the approach before writing code.

### Iterative Development
- Make small, incremental changes that can be reviewed easily.
- Commit logical units of work separately.
- Test each change before moving to the next.

### Communication
- Explain what you're about to do before doing it.
- If a task is taking longer than expected, communicate progress and blockers.
- When presenting options, explain pros/cons without time estimates.

---

## Bash Guidelines

### Output Buffering
- **DO NOT** pipe output through `head`, `tail`, `less`, or `more` when monitoring or checking command output.
- **DO NOT** use `| head -n X` or `| tail -n X` to truncate output—these cause buffering problems.
- Instead, let commands complete fully, or use `--max-lines` flags if the command supports them.
- For log monitoring, prefer reading files directly rather than piping through filters.

### Command Execution
- Run commands directly without pipes when possible.
- Use command-specific flags to limit output (e.g., `git log -n 10` instead of `git log | head -10`).
- Avoid chained pipes that can cause output to buffer indefinitely.
- Always quote file paths that contain spaces.

---

## Code Quality Standards

### Error Handling
- Only add error handling at system boundaries (user input, external APIs).
- Trust internal code and framework guarantees.
- Don't add validation for scenarios that can't happen.

### Testing
- Write tests first when implementing new features (TDD approach gives leverage).
- Run tests after making changes to verify correctness.
- Don't skip tests to save time.

### Security
- Never commit secrets, API keys, or credentials.
- Use environment variables for sensitive configuration.
- Be cautious with user input—validate at boundaries.

---

## Git Workflow

### Commits
- Write clear, concise commit messages that explain the "why."
- Commit logical units of work—not too large, not too granular.
- Don't amend commits unless explicitly asked.
- Stage specific files rather than using `git add .` or `git add -A`.

### Branches
- Create feature branches for new work.
- Keep branches focused on a single feature or fix.
- Push regularly to maintain backup and enable collaboration.

---

## Stack-Specific Guidelines

### Vercel (Frontend, API Routes, Cron)
- Use Next.js App Router conventions.
- Place API routes in `app/api/` directory.
- Configure cron jobs in `vercel.json`.
- Use Edge Runtime where appropriate for performance.
- Leverage Vercel's built-in analytics and monitoring.

### Supabase (Auth, Database)
- Use Row Level Security (RLS) policies for data access control.
- Keep database schema in version control via migrations.
- Use Supabase client for auth flows—don't roll custom auth.
- Prefer Supabase's built-in auth providers over custom implementations.

### TypeScript
- Use strict mode.
- Prefer explicit types over `any`.
- Let TypeScript infer types where obvious.
- Don't add redundant type annotations.

---

## Skills Integration

This project uses [skills.sh](https://skills.sh) for reusable agent capabilities.

### Adding Skills
```bash
npx skills add <owner/repo> --skill <skill-name>
```

### Finding Relevant Skills
```bash
npx skills add https://github.com/vercel-labs/skills --skill find-skills
```

### Recommended Skills
- `vercel-react-best-practices` - React patterns for Vercel deployment
- `web-design-guidelines` - UI/UX best practices
- `frontend-design` - Frontend architecture patterns

---

## Anti-Patterns to Avoid

### Code Bloat
- Don't create helpers, utilities, or abstractions for one-time operations.
- Don't design for hypothetical future requirements.
- Don't use feature flags or backwards-compatibility shims when you can just change the code.

### Over-Documentation
- Don't add `// removed` comments for deleted code.
- Don't rename unused variables to `_var` for backwards compatibility.
- Don't re-export types or functions that are no longer used.
- If something is unused, delete it completely.

### Scope Creep
- Don't "improve" adjacent code while fixing a bug.
- Don't refactor files you're only reading.
- Don't add logging, metrics, or observability unless asked.
- Don't upgrade dependencies unless directly relevant to the task.

---

## Leverage Patterns

### Declarative Over Imperative
- Give success criteria rather than step-by-step instructions.
- Let the agent loop until goals are met.
- Write naive algorithms first, then optimize while preserving correctness.

### Test-Driven Development
- Write tests first as success criteria.
- Use tests to verify correctness automatically.
- Tests provide leverage by enabling confident iteration.

### Browser/Tool Integration
- Use MCPs and tool integrations for interactive tasks.
- Let agents verify their own work through tooling.

---

## Project Structure

```
├── app/                    # Next.js App Router
│   ├── api/               # API routes
│   ├── (auth)/            # Auth-related pages
│   └── (dashboard)/       # Protected pages
├── components/            # React components
├── lib/                   # Utility functions
│   ├── supabase/         # Supabase client config
│   └── utils/            # General utilities
├── public/               # Static assets
├── supabase/             # Supabase config
│   └── migrations/       # Database migrations
├── .env.local            # Environment variables (not committed)
├── CLAUDE.md             # This file
└── vercel.json           # Vercel configuration
```

---

## Environment Variables

Required variables (to be set in Vercel/local `.env.local`):

```
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
```

---

## Quick Reference

| Task | Command |
|------|---------|
| Start dev server | `npm run dev` |
| Run tests | `npm test` |
| Build for production | `npm run build` |
| Deploy | `vercel` |
| Generate Supabase types | `npx supabase gen types typescript` |
| Create migration | `npx supabase migration new <name>` |
| Push migration | `npx supabase db push` |
