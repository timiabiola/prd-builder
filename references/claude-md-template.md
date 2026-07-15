# Claude.md Template

## Overview

Every project should include a `Claude.md` file at the root of the repository. This file provides context and instructions for Claude AI when working on the project.

**Based on Anthropic's Official Best Practices:**
- Keep content concise and human-readable (< 300 lines recommended)
- Use short, declarative bullet points
- Write for Claude, not onboarding documentation
- Use emphasis markers like "IMPORTANT" or "YOU MUST" for critical rules
- Iterate and refine based on effectiveness

**References:**
- [Anthropic Claude Code Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices)
- [How Anthropic Teams Use Claude Code](https://www-cdn.anthropic.com/58284b19e702b49db9302d5b6f135ad8871e7658.pdf)

---

## Template

```markdown
# Claude.md - [Project Name]

## Project Overview

[1-2 sentence description of what this project does]

**Tech Stack:** [e.g., Next.js 14, TypeScript, Tailwind, shadcn/ui, Supabase]

---

## Quick Reference

### Commands
```bash
pnpm dev          # Development server
pnpm build        # Production build
pnpm test         # Run tests
pnpm test:e2e     # Playwright E2E tests
pnpm lint         # Lint code
```

### Key Files
| File | Purpose |
|------|---------|
| `docs/prd.md` | Product requirements (spec input to superpowers) |
| `docs/tasks.md` | Single source of truth for milestones and work items |
| `docs/planning.md` | Architecture, tech stack, dev workflow |
| `docs/checkpoint.md` | Progress tracking (updated before commits) |
| `docs/plans/` | Per-feature execution plans (written by `superpowers:writing-plans`) |

---

## MCP Tools Configuration

### Docker Gateway Tools (mcp__MCP_DOCKER__*)

| Tool | Purpose | When to Use |
|------|---------|-------------|
| **Playwright** | Testing | E2E, integration tests (MANDATORY per sprint) |
| **Context7** | Documentation | Planning, debugging, implementation (MANDATORY) |
| **Resend** | Email | Transactional emails |

#### Playwright MCP - E2E Testing (REQUIRED)

**YOU MUST run Playwright E2E tests after every development sprint.**

**CRITICAL: Docker MCP runs in a container - localhost won't work!**

The Playwright MCP runs inside a Docker container, which means it cannot access `localhost` on your machine. You MUST use your machine's network IP address.

**Get Your Network Address:**
```bash
# macOS
ipconfig getifaddr en0

# Linux
hostname -I | awk '{print $1}'

# Windows
ipconfig | findstr IPv4
```

**Start Dev Server on Network:**
```bash
pnpm dev --host
# or
npm run dev -- --host
```

**Network Address Usage:**
```
USE:     http://192.168.x.x:3000  (your actual network IP)
NOT:     http://localhost:3000
NOT:     http://127.0.0.1:3000
```

**Pass Rate Requirement: 95% MINIMUM**
- Tests MUST achieve 95% pass rate before marking tasks complete
- If < 95%: fix issues and re-run until threshold met
- Document results in `docs/checkpoint.md`

**Usage:**
```typescript
// First get your network IP, then use it:
mcp__MCP_DOCKER__browser_navigate({ url: 'http://192.168.1.100:3000' })
mcp__MCP_DOCKER__browser_snapshot({})
mcp__MCP_DOCKER__browser_click({ element: 'Login', ref: 'button[0]' })
mcp__MCP_DOCKER__browser_take_screenshot({ filename: 'test.png' })
```

#### Context7 MCP - Documentation (REQUIRED)

**YOU MUST use Context7 during planning and debugging phases for current documentation.**

**When to Use:**
- Planning: Query library docs before architecture decisions
- Debugging: Get current API patterns when troubleshooting
- Implementation: Verify correct dependency usage

**Usage:**
```typescript
mcp__MCP_DOCKER__resolve-library-id({ libraryName: 'next.js' })
mcp__MCP_DOCKER__get-library-docs({
  context7CompatibleLibraryID: '/vercel/next.js',
  topic: 'app router',
  tokens: 10000
})
```

### Local MCP Tools (mcp__shadcn__*)

| Tool | Purpose | When to Use |
|------|---------|-------------|
| **shadcn/ui** | UI Components | Building UI with ShadCN |

```typescript
mcp__shadcn__search_items_in_registries({ registries: ['@shadcn'], query: 'button' })
mcp__shadcn__view_items_in_registries({ items: ['@shadcn/button'] })
mcp__shadcn__get_item_examples_from_registries({ registries: ['@shadcn'], query: 'button-demo' })
```

---

## Development Workflow (Superpowers)

**YOU MUST follow this pattern when starting any new development work in this project.** The 4 source-of-truth docs tell you where the project stands; superpowers turns the next unstarted work into executable code.

### The 6-step spine

1. **Read the 4 source-of-truth docs first** to understand where we are:
   - `docs/prd.md` — what we're building (the spec)
   - `docs/tasks.md` — what's left and what's in progress
   - `docs/planning.md` — architecture and tech stack
   - `docs/checkpoint.md` — current status, recent work, blockers
2. **Plan the next stage** — invoke `superpowers:writing-plans` against the next unstarted P0/P1 task. Superpowers creates and owns the plan, saved to `docs/plans/YYYY-MM-DD-<feature>.md`.
3. **Execute** — invoke `superpowers:executing-plans` against the plan, or `superpowers:subagent-driven-development` for multi-task plans where independent subagents implement and review in series.
4. **TDD** — every plan follows Red → Verify Red → Green → Verify Green → Refactor via `superpowers:test-driven-development`. No production code without a failing test first.
5. **Verify** — `superpowers:verification-before-completion` gates every "done" claim with fresh test/build evidence. No "passing" claim without running it.
6. **Track** — update `docs/tasks.md` (check off items, add discovered tasks) and `docs/checkpoint.md` (status, blockers, change log) as work lands. **Do NOT use** `TaskCreate`/`TaskUpdate` — edit the markdown files directly.

### Why this matters

Without this pattern, a new session has no idea where the project stands or what to do next. With it, any session can be productive within minutes of opening — and the work it produces is structured, tested, and verified before it's claimed done.

---

## Sprint Workflow (MANDATORY)

### Per-Sprint Requirements

**IMPORTANT: A sprint is NOT complete until ALL of the following are done:**

1. **E2E Testing**
   - [ ] Run full test suite via Playwright MCP
   - [ ] Achieve 95%+ pass rate
   - [ ] Fix any failing tests
   - [ ] Document results in `docs/checkpoint.md`

2. **Documentation Updates**
   - [ ] Update `docs/checkpoint.md` with progress and test results
   - [ ] Update `docs/tasks.md` — check off completed items, add discovered tasks
   - [ ] Update `docs/planning.md` with any architectural changes

3. **Commit**
   - [ ] All changes committed with proper message
   - [ ] Sprint marked complete

4. **Pre-Push Requirements (MANDATORY)**
   - [ ] `docs/checkpoint.md` updated with current status
   - [ ] `docs/tasks.md` reflects accurate completion (edited in-file)
   - [ ] `docs/planning.md` documents any architecture changes

### Pre-Push Checklist

**YOU MUST update these files BEFORE every `git push`:**

```bash
# Before pushing to remote:
1. Update docs/checkpoint.md (progress, test results)
2. Update docs/tasks.md (check off completed items)
3. Update docs/planning.md (architectural changes)
4. git add -A
5. git commit -m "feat: description

Updated docs/checkpoint.md, docs/planning.md"
6. git push origin <branch>
```

### Per-Session Startup

**Before Starting any new work:**
1. Read `docs/prd.md`, `docs/tasks.md`, `docs/planning.md`, `docs/checkpoint.md` (the 4 source-of-truth docs)
2. Invoke `superpowers:writing-plans` to plan the next P0/P1 task — superpowers writes the plan to `docs/plans/YYYY-MM-DD-<feature>.md`
3. Invoke `superpowers:executing-plans` (or `superpowers:subagent-driven-development` for multi-task plans)

**During Development:**
1. Follow `superpowers:test-driven-development` discipline (no production code without failing test first)
2. Use MCP tools (Context7 for library docs, Playwright for E2E)
3. Follow established codebase patterns

**Before Committing:**
1. Run E2E tests (95% pass rate required) and verify per `superpowers:verification-before-completion`
2. Update `docs/checkpoint.md` (REQUIRED)
3. Update `docs/tasks.md` and `docs/planning.md` as needed

---

## Project Structure

```
[project-root]/
├── src/
│   ├── app/              # Next.js app router
│   ├── components/       # React components
│   │   ├── ui/           # shadcn/ui components
│   │   └── ...
│   └── lib/              # Utilities
├── tests/                # Test files
└── docs/                 # Documentation
```

---

## Conventions

### Code Style
- TypeScript strict mode
- Functional components with hooks
- Server Components by default

### Naming
- Components: PascalCase
- Files: kebab-case
- Functions: camelCase

### Commits
```
type(scope): description

Updated docs/checkpoint.md
```
Types: feat, fix, docs, style, refactor, test, chore

---

## Important Notes

- NEVER hardcode API keys or secrets
- ALWAYS update `docs/checkpoint.md` before commits
- ALWAYS read the 4 source-of-truth docs (`docs/prd.md`, `docs/tasks.md`, `docs/planning.md`, `docs/checkpoint.md`) before starting new work
- ALWAYS invoke `superpowers:writing-plans` to plan the next stage — don't freelance
- 95% E2E test pass rate required per sprint, verified via `superpowers:verification-before-completion`
- Use Context7 for current documentation in planning/debugging
```

---

## Guidelines

### Anthropic Best Practices (FOLLOW THESE)

Based on [official Anthropic documentation](https://www.anthropic.com/engineering/claude-code-best-practices):

1. **Keep it concise** - Less than 300 lines, shorter is better
2. **Use declarative bullets** - Not narrative paragraphs
3. **Write for Claude** - Not onboarding documentation
4. **Include emphasis markers** - "IMPORTANT", "YOU MUST", "CRITICAL"
5. **Iterate on effectiveness** - Refine what produces best results
6. **Universal applicability** - Only include broadly relevant instructions

### What to Include (Per Anthropic)

- Common bash commands with descriptions
- Core utility functions and frequently used modules
- Code style guidelines
- Testing instructions (including MCP testing requirements)
- Repository workflow practices
- Project-specific quirks or warnings

### What NOT to Include

- Overly specific one-time instructions
- Long narrative explanations
- Information that won't apply across sessions
- Database schema details (unless critical)

### MCP Tools Section (REQUIRED)

Every Claude.md MUST include:

1. **Playwright MCP via Docker** - E2E testing requirements
   - 95% pass rate mandate
   - host.docker.internal usage
   - Sprint testing workflow

2. **Context7 MCP via Docker** - Documentation requirements
   - Planning phase usage
   - Debugging phase usage
   - Current documentation queries

3. **Sprint documentation updates**
   - `docs/checkpoint.md`, `docs/tasks.md`, `docs/planning.md` requirements

### Sprint Completion Checklist (REQUIRED)

Every Claude.md should include a sprint completion checklist:

```markdown
**Sprint Completion Requirements:**
- [ ] E2E tests pass at 95%+ rate (verified per `superpowers:verification-before-completion`)
- [ ] `docs/checkpoint.md` updated with test results
- [ ] `docs/tasks.md` updated — checkboxes flipped, new tasks added
- [ ] `docs/planning.md` updated with architectural changes
- [ ] All changes committed
- [ ] Sprint marked complete
```

This ensures consistent project management and accurate progress tracking.
