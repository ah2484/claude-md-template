# Project Guidelines for Codex

> A reusable AGENTS.md template for AI-assisted development projects using [OpenAI Codex](https://openai.com/index/introducing-codex/).

---

## Core Principles

### Simplicity Over Complexity
- **Avoid over-engineering.** Only make changes that are directly requested or clearly necessary.
- Do not add features, refactor code, or make "improvements" beyond what was asked.
- A bug fix doesn't need surrounding code cleaned up. A simple feature doesn't need extra configurability.
- Prefer 3 similar lines of code over a premature abstraction.
- The right amount of complexity is the minimum needed for the current task.

### Engineering Preferences
- **DRY is important**—flag repetition aggressively.
- **Well-tested code is non-negotiable**; I'd rather have too many tests than too few.
- Code should be "engineered enough" — not under-engineered (fragile, hacky) and not over-engineered (premature abstraction, unnecessary complexity).
- Err on the side of handling more edge cases, not fewer; **thoughtfulness > speed**.
- Bias toward explicit over clever.

### Do Not Assume
- If requirements are ambiguous, prefer the most conservative interpretation.
- Surface inconsistencies in comments on the PR rather than guessing.
- When multiple valid approaches exist, choose the simplest one that satisfies the requirements.
- If something seems wrong or confusing, leave a comment explaining the concern rather than proceeding silently.

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

## Working Agreements

### Task Execution
- Break complex tasks into clear, sequential steps.
- Identify potential risks or edge cases upfront.
- Make small, incremental commits that can be reviewed easily.
- Each commit should represent a logical unit of work.
- Test each change before moving to the next.

### PR Quality
- Keep PRs focused on a single concern—don't conflate separate issues.
- Write clear PR descriptions that explain the "why" behind changes.
- If the task touches multiple concerns, note what was and wasn't addressed.
- When making adjustments (spacing, typography, layout, etc.), apply them precisely without adding unrequested changes.

### When In Doubt
- Prefer doing less over doing more—it's easier to add than to remove.
- Leave PR comments explaining decisions, tradeoffs, or areas of uncertainty.
- Don't assume priorities on timeline or scale.

---

## Repository Expectations

### Code Review Checklist

When reviewing or writing code, evaluate against these criteria:

**Architecture:**
- Component boundaries and separation of concerns.
- Dependency graph and coupling.
- Data flow patterns and potential bottlenecks.
- Security architecture (auth, data access, API boundaries).

**Code Quality:**
- DRY violations—be aggressive here.
- Error handling patterns and missing edge cases.
- Technical debt hotspots.
- Over-engineering or under-engineering.

**Tests:**
- Test coverage gaps (unit, integration, e2e).
- Missing edge case coverage.
- Untested failure modes and error paths.

**Performance:**
- N+1 queries and database access patterns.
- Memory-usage concerns.
- Caching opportunities.

---

## Shell Commands

### Avoid output buffering issues
- **DO NOT** pipe output through `head`, `tail`, `less`, or `more`.
- **DO NOT** use `| head -n X` or `| tail -n X` to truncate output.
- Let commands complete fully, or use command-specific flags (e.g., `git log -n 10` instead of `git log | head -10`).
- Always quote file paths that contain spaces.

---

## Code Quality Standards

### Error Handling
- Only add error handling at system boundaries (user input, external APIs).
- Trust internal code and framework guarantees.
- Don't add validation for scenarios that can't happen.
- But err on the side of handling more edge cases at boundaries—thoughtfulness over speed.

### Testing
- **Well-tested code is non-negotiable.**
- Write tests first when implementing new features (TDD approach).
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
├── AGENTS.md             # This file
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

### Vercel Environment Variable Gotcha

Vercel environment variables often contain trailing `\n` (newline) characters from copy-paste. This causes silent auth failures, API 401s, and other hard-to-debug issues.

**Always `.trim()` environment variables when reading them:**

```typescript
const apiKey = process.env.MY_API_KEY?.trim();
```

If an API key or secret is failing only in production (but works locally), a trailing newline in the Vercel env var is the most likely cause.

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
