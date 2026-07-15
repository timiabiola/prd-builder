# Testing & Sprint Documentation Workflow

Testing discipline is governed by `superpowers:test-driven-development` (every feature follows Red → Verify Red → Green → Verify Green → Refactor) and gated by `superpowers:verification-before-completion` (no "passing" claim without fresh evidence). The Playwright CLI run below is the E2E verification step inside that discipline, not a substitute for it.

## Quality Gate: Playwright CLI E2E Tests

Every development sprint concludes with E2E testing before marking tasks complete.

Use **Playwright CLI** (`npx playwright test`) for all end-to-end testing. This is the CLI tool, not an MCP.

### Pass Rate Requirement: 95% MINIMUM

- All E2E tests must achieve **95% pass rate** before finalizing sprint tasks
- If < 95%, fix issues and re-run until threshold is met
- Document all test results in `docs/checkpoint.md`

### Testing Workflow Per Sprint

1. Run full E2E test suite: `npx playwright test`
2. Capture test results and pass rate percentage
3. If < 95% pass rate:
   - Identify failing tests
   - Use **Context7 MCP** (Docker) to fetch latest library docs for debugging
   - Debug and fix issues
   - Re-run until 95% threshold achieved
4. Document final pass rate in `docs/checkpoint.md`
5. Only then mark sprint tasks as complete

## MCP Tools for Development

### Context7 MCP (Docker) — Documentation & Debugging

Use whenever building new features or debugging issues:

```typescript
// Resolve library ID first
mcp__MCP_DOCKER__resolve-library-id({ libraryName: 'next.js' })

// Get current documentation
mcp__MCP_DOCKER__get-library-docs({
  context7CompatibleLibraryID: '/vercel/next.js',
  topic: 'app router',
  tokens: 10000
})
```

**When to use:**
- Before architecture decisions (get latest API patterns)
- When debugging (verify correct usage)
- Before major code changes (ensure patterns are current)

### Stripe MCP (Docker) — Payment Implementation

Use for all Stripe integration work (checkout, subscriptions, webhooks).

### Convex MCP — Database Operations

Use for all Convex work: creating mutations, functions, queries, schema changes.

## Sprint Documentation Updates

At the end of every sprint, ALL documentation must be updated:

### 1. `docs/checkpoint.md` (before every commit)
- Current progress percentage
- Test pass rate achieved
- Completed items (checked boxes)
- Blockers encountered
- Active plan link (`docs/plans/...`)
- Next actions

### 2. `docs/tasks.md` (edit in-file)
- Check off completed tasks (flip `[ ]` → `[x]`)
- Move in-progress markers (`[~]`)
- Flag blockers (`[!]`)
- Add newly discovered tasks
- Do NOT use `TaskCreate`/`TaskUpdate` — edit the markdown directly

### 3. `docs/planning.md`
- Document tech stack changes
- Update integration points
- Record architectural decisions
- Note any pivots or scope changes

## Sprint Completion Checklist

```markdown
- [ ] E2E tests pass at 95%+ via `npx playwright test` (verified per `superpowers:verification-before-completion`)
- [ ] `docs/checkpoint.md` updated with test results
- [ ] `docs/tasks.md` updated — checkboxes flipped, new tasks added
- [ ] `docs/planning.md` updated with any architectural changes
- [ ] All changes committed with proper commit message
```

## Pre-Push Requirements

Before every `git push`:

1. `docs/checkpoint.md` reflects current progress
2. `docs/tasks.md` shows accurate completion status
3. `docs/planning.md` documents any architectural changes
4. All tests passing (95%+ E2E rate)
