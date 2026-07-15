---
name: prd-builder
description: Orchestrates a full superpowers-integrated project bootstrap. When the idea or next step isn't clear it starts with `superpowers:brainstorming` (produces the design spec); it asks for the tech stack when not provided (defaulting to the standard stack — Convex, Clerk/WorkOS, Resend, Twilio, Next.js/TypeScript, Framer Motion, PostHog, RevenueCat, AI SDKs); it generates the PRD and full scaffold (`docs/prd.md`, `docs/tasks.md`, `docs/planning.md`, `docs/checkpoint.md`, `CLAUDE.md` at root); and it ends by offering to invoke `superpowers:writing-plans` (after user confirmation) to produce the first sprint's execution plan. By the end of the run you have the 4 docs + CLAUDE.md + the brainstorming spec + (on confirmation) the first sprint plan. Uses `docs/tasks.md` as the single source of truth for tasks (no native task list). Use when bootstrapping a new project, building a PRD, planning app architecture, or when the user says "create a PRD", "plan this app", "build requirements for", "bootstrap this project". Includes CLINIC MODE for healthcare clients. Always use this skill when starting a new project from scratch — even if the user doesn't explicitly say "PRD".
---

# PRD Builder Skill

Build comprehensive Product Requirement Documents with analytics frameworks, persistent task management, and complete project scaffolding — ensuring every app can demonstrate measurable business impact.

## Core Philosophy

Every app must answer: **"How do we prove this is working?"** This skill bakes KPIs, attribution tracking, conversion funnels, and proof dashboards into the PRD from day one.

## How This Skill Orchestrates Superpowers

This skill is the **controller** for a superpowers-integrated bootstrap. It runs three phases in one continuous flow — you don't hand off and stop, you drive the whole arc:

| Phase | Skill | Runs when | Output |
|---|---|---|---|
| **1. Brainstorm** | `superpowers:brainstorming` | The idea, next step, or rough shape isn't clear | Approved design spec at `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md` |
| **2. PRD + Scaffold** | `prd-builder` (this skill) | Always | `docs/prd.md` + `docs/tasks.md`, `docs/planning.md`, `docs/checkpoint.md`, `CLAUDE.md` |
| **3. First plan** | `superpowers:writing-plans` | End of the run, **after the user confirms** they want it now | First sprint's execution plan at `docs/superpowers/plans/YYYY-MM-DD-<feature>.md` |

**By the end of this run you have all of:** the brainstorming spec (if Phase 1 ran), the 4 source-of-truth docs, `CLAUDE.md`, and the first sprint's plan ready to execute.

**Important handoff note:** `superpowers:brainstorming` normally treats "invoke writing-plans" as its terminal state. When brainstorming runs *inside this skill*, it does NOT jump straight to writing-plans — after the spec is written and the user approves it, **return here** to build the PRD + scaffold (Phase 2). `superpowers:writing-plans` is invoked only once, as the final step (Phase 3), so the first plan is grounded in both the spec and the PRD.

If the request is already clear and concrete (e.g. "Build a Stripe-powered invoice automation SaaS for accounting firms at $49/mo"), skip Phase 1 — this skill's requirements gathering (Step 1) is enough planning to drive the PRD.

## PRD Generation Workflow

### Step 0 (Phase 1): Brainstorm When the Idea Isn't Clear

**If the idea, the next step, or the rough shape of the project isn't clear, invoke `superpowers:brainstorming` now** — before gathering requirements. This is the default for vague or exploratory requests ("I'm thinking about building X but not sure if...", "help me figure out what to build", requests that describe a problem but not a product).

Run the full brainstorming conversation: it explores intent, proposes 2-3 approaches, presents the design in sections, writes an approved spec to `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`, and gets user sign-off on it.

**Then return here.** Do NOT let brainstorming hand straight off to `superpowers:writing-plans` — that handoff is deferred to Phase 3. Once the spec is written and approved, continue to Step 1 (tech stack) and generate the PRD + scaffold. The spec becomes an input to the PRD.

**Skip Phase 1** only when the request is already clear and the product shape is obvious (e.g. "Build a Stripe-powered invoice automation SaaS for accounting firms at $49/mo"). In that case Step 1's requirements gathering is enough planning.

### Step 1: Gather Requirements (incl. Tech Stack)

Ask the client/stakeholder:

1. **Business Context** — Problem, end users, 30/60/90-day success criteria
2. **Monetization** — Revenue model, conversion events, target CLV
3. **Marketing Channels** — Ad platforms (Meta, Google, TikTok, Reddit), offline channels, attribution setup
4. **Mode** — Healthcare/clinic? → CLINIC MODE. General business? → STANDARD MODE.
5. **Tech Stack** — If the user didn't specify a stack, propose the **Default Tech Stack** below and confirm it. Ask the decision questions for anything the default leaves open (platform, compliance, which optional services apply). Record the confirmed stack — it fills `docs/planning.md` §3 (Technology Stack) and `CLAUDE.md`'s Tech Stack line.

### Default Tech Stack & Service Decisions

When the stack isn't provided, default to this and confirm with the user. **Ask** about anything a service's inclusion depends on — don't assume.

| Concern | Default | Ask before including / swapping |
|---|---|---|
| **Web framework** | Next.js + TypeScript | — (default for web) |
| **Backend + file storage** | Convex | — (default) |
| **Auth + waitlist** | Clerk (also covers waitlist if needed) | "Do you need auth? A waitlist?" |
| **Compliance-grade auth** | WorkOS **instead of/alongside Clerk** | "Is HIPAA or SOC 2 Type II a high-importance requirement?" → yes = WorkOS. (CLINIC MODE implies HIPAA — flag WorkOS by default there.) |
| **Email** | Resend | "Do you need transactional/marketing email?" |
| **SMS / text** | Twilio | "Do you need text messaging?" |
| **Animation / graphics** | Framer Motion | — (default for web) |
| **Analytics** | PostHog | — (default; this skill's analytics framework assumes product analytics) |
| **Paywall / subscriptions** | RevenueCat | "Is there a paywall or subscription?" |
| **AI features** | Gemini, Anthropic, and/or OpenAI SDKs | "Which AI features do you need, and any model-provider preference?" — include only the SDKs actually used |

**Platform swap (ask first: "Web, iOS only, or iOS + Android?")**

- **Web** → keep Next.js + TypeScript + Framer Motion.
- **iOS only** → swap the web front-end for **native iOS / SwiftUI** (Convex, Clerk/WorkOS, Resend, Twilio, PostHog, RevenueCat still apply).
- **iOS + Android** → use **React Native** for the client.

Whichever services the user confirms, list only those in `docs/planning.md` and `CLAUDE.md` — don't carry unused defaults into the generated docs.

### Step 2: Generate PRD Structure

Use `references/prd-template.md` as the foundation. Every PRD includes:

1. Executive Summary
2. User Personas & Journeys
3. Core Features (P0/P1/P2 prioritized)
4. **Analytics & Data Requirements** (mandatory — see Step 3)
5. Technical Architecture
6. Success Metrics & KPIs
7. Timeline & Milestones
8. Risks & Mitigations
9. **Responsive UI Requirements** (read `references/responsive-ui.md` for the template)

### Step 3: Build Analytics Framework

The critical differentiator. Read `references/analytics-framework.md` for the complete data spine.

**Funnel Metrics (Universal)**
- Acquisition: Leads by source, cost per lead, response time
- Conversion: Lead → consult → sale rates
- Revenue: Baseline, incremental by source, revenue by tier
- Retention: Churn, repeat purchase frequency, NPS

**Ad Platform Integrations** — Meta, Google, TikTok, Reddit APIs. See `references/ad-platform-apis.md`.

**Attribution** — UTM standards, first/last/multi-touch models, cross-device, offline conversion tracking.

### Step 4: CLINIC MODE (Healthcare Clients)

Read `references/clinic-mode.md` for full specification. Adds:

- Patient journey tracking (lead → consult → program → membership)
- Program tier tracking (Core $2K-$5K, Premium $5K-$15K, Elite $15K+)
- Healthcare metrics: PAC, PLV, completion rates, referral tracking
- Compliance: HIPAA, BAA, consent language, medical disclaimers
- Verticals: GLP-1/weight loss, hormone therapy, TRT, medspas, metabolic/longevity

### Step 5: Define Dashboard Requirements

Every PRD specifies a "Proof Dashboard":

1. **Headline Metrics** (3-5 max) — what the client cares about most, vs baseline
2. **Funnel Visualization** — stage-by-stage conversion rates, drop-off identification
3. **Channel Attribution** — revenue/conversions by source, ad spend ROI
4. **Time-Series Trends** — weekly/monthly performance, before/after comparison

### Step 6: Finalize PRD → write `docs/prd.md`

Ensure the document includes:
- Clear P0/P1/P2 prioritization
- Technical feasibility notes (reflect the **confirmed tech stack** from Step 1 in the Technical Architecture section)
- Data schema requirements
- API integration specifications
- Success criteria with specific numeric targets

**Write the finished PRD to `docs/prd.md`** — this is a required output of the run. It is the spec input to `superpowers:writing-plans` in Phase 3, and one of the four source-of-truth docs every later session reads. Do not leave the PRD only in-conversation; the file must exist on disk before scaffolding continues.

---

## Project Scaffolding

After the PRD is complete, generate the full project scaffold.

### Step 7: Generate `docs/tasks.md`

`docs/tasks.md` is the single source of truth for work. **Do not use** Claude Code's native task list (`TaskCreate`/`TaskUpdate`), `CLAUDE_CODE_TASK_LIST_ID`, or `~/.claude/tasks/...`. Those are removed from this workflow.

Use `references/tasks-template.md` as the structure. Required:

1. **Milestones**: Foundation → Core Features → Analytics → Dashboard → Testing → Launch
2. **Per-milestone**: Timeline, Definition of Done, Dependencies, Blockers
3. **Status legend** at top of the file:
   - `[ ]` not started
   - `[~]` in progress
   - `[x]` complete
   - `[!]` blocked
4. **P0/P1/P2 priority labels** on every task
5. **Backlog** and **Notes** (assumptions, risks, out of scope) at the bottom

Tasks are checked off in-file as they complete; the file is committed alongside code changes. No native task list, no `.claude/settings.json` env vars, no `~/.claude/tasks/` plumbing.

### Step 8: Generate `docs/planning.md`

Read `references/planning-template.md` for structure. Required sections:

1. Vision Statement
2. Architecture Overview (system diagram, data flow)
3. Technology Stack with rationale
4. Integration Points (APIs, webhooks, data sources)
5. Security & Compliance (HIPAA if clinic mode)
6. Scalability Considerations
7. **Development Workflow** — the 6-step superpowers spine (read 4 docs → `superpowers:writing-plans` → `superpowers:executing-plans` → TDD → verify → update tasks/checkpoint)

### Step 9: Generate `docs/checkpoint.md`

Read `references/checkpoint-template.md`. Updated before every commit:

1. Current Status (progress %, current phase)
2. Tasks summary (snapshot of `docs/tasks.md`)
3. Completed / In Progress / Blockers
4. Active plan link (current `docs/superpowers/plans/...` file — set to the first sprint plan in Phase 3)
5. Next Actions
6. Change Log

### Step 10: Generate `CLAUDE.md` (project root)

CLAUDE.md stays at the **project root** so Claude Code auto-loads it. It must include:

1. Project overview and tech stack
2. **Key Files** table pointing at `docs/prd.md`, `docs/tasks.md`, `docs/planning.md`, `docs/checkpoint.md`
3. MCP tool configuration (Playwright, Context7, shadcn/ui)
4. **Development Workflow (Superpowers)** section — the 6-step pattern that triggers on every future session
5. Sprint workflow and commit conventions

Read `references/claude-md-template.md` for the template.

### Step 11 (Phase 3): Write the First Sprint Plan

Now that the PRD and 4 docs exist, offer to produce the execution plan for the **first sprint** — the top unstarted P0 feature/milestone in `docs/tasks.md`.

0. **Confirm first.** Ask the user: *"Scaffold is complete. Want me to write the first sprint's plan now with `superpowers:writing-plans`, or stop here at the docs?"* Only proceed to the steps below if they say yes. If they stop here, skip to the confirmations (steps 4–5) and the end-of-run announcement (noting the plan is not yet written).
1. **Pick the first sprint's scope** — the highest-priority P0 milestone from `docs/tasks.md` (the Foundation/first Core Feature). If the PRD is large, plan only that first slice, not the whole product.
2. **Invoke `superpowers:writing-plans`** with the PRD (and the brainstorming spec, if Phase 1 ran) as input. It writes the plan to `docs/superpowers/plans/YYYY-MM-DD-<feature>.md` with bite-sized TDD tasks, and offers its execution-handoff choice (subagent-driven vs inline). **Do not auto-execute** — stop at the plan and let the user choose when to start building.
3. **Link the plan** — set the "Active plan link" in `docs/checkpoint.md` to the new plan file, and note the first sprint as in-progress in `docs/tasks.md`.
4. **Confirm `CLAUDE.md` (root) contains the "Development Workflow (Superpowers)" section** so the pattern triggers on every future session open.
5. **Confirm `docs/planning.md` has a "Development Workflow" section** mirroring the same 6-step spine.

**End-of-run announcement** to the user:
> Bootstrap complete. You now have:
> - The brainstorming spec (`docs/superpowers/specs/...`, if we brainstormed)
> - The PRD (`docs/prd.md`) and the 3 tracking docs (`docs/tasks.md`, `docs/planning.md`, `docs/checkpoint.md`)
> - `CLAUDE.md` at the root (auto-loaded next session)
> - The **first sprint's plan** (`docs/superpowers/plans/...`), ready to execute — *if you asked me to write it; otherwise invoke `superpowers:writing-plans` when you're ready*
>
> Next: execute the plan with `superpowers:executing-plans` (or `superpowers:subagent-driven-development`), verify per `superpowers:verification-before-completion`, then update `docs/tasks.md` + `docs/checkpoint.md`. Each later sprint repeats: read the 4 docs → `superpowers:writing-plans` for the next P0/P1 → execute → verify → update docs.

---

## Design & UI Standards

### Frontend Design Skill

Use `/frontend-design` skill for all UI decisions. Default recommendation: **clinical elegance light + editorial style**.

- **Component library**: HeroUI (not shadcn) + Tailwind CSS
- **Animations**: Framer Motion — cinematic, exquisite feel (0.8-1.2s durations)
- **Novel UI**: Source components from https://21st.dev/community/components to break typical patterns
- **Brand**: White and gold primary palette (unless client specifies otherwise)
- **Typography**: Massive serif headings, asymmetric layouts, bento grids

### Responsive Architecture

Desktop-first with comprehensive mobile optimization. Read `references/responsive-ui.md` for hooks, patterns, Tailwind strategy, and performance budgets.

After any UI component is built, invoke the `mobile-ui-optimizer` agent to audit mobile experience.

---

## Quality Gates

### Testing

Use **Playwright CLI** (`npx playwright test`) for all E2E testing. 95% pass rate required before any sprint is complete.

Use **Context7 MCP** (Docker) when building features or debugging. Use **Stripe MCP** (Docker) for payment implementation. Use **Convex MCP** for all database operations.

Read `references/testing-workflow.md` for the complete testing and sprint documentation process.

### Sprint Documentation (after every sprint)

1. Update `docs/checkpoint.md` (progress, test results, blockers)
2. Update `docs/tasks.md` — check off completed items, add new tasks discovered during the sprint
3. Update `docs/planning.md` with any architectural changes
4. Confirm verification per `superpowers:verification-before-completion` before claiming the sprint complete
5. No sprint is complete until docs are current

---

## Subagent Coordination

Subagents coordinate through `docs/tasks.md` (read-write) and whatever plan file `superpowers:writing-plans` produced for the current feature. The recommended pattern for multi-task work is `superpowers:subagent-driven-development`, which:

- Reads the plan once in the controller, then dispatches one implementer subagent per task
- Follows each implementer with a spec-compliance reviewer, then a code-quality reviewer
- Has implementers run TDD per `superpowers:test-driven-development`
- Has the controller update `docs/tasks.md` and `docs/checkpoint.md` as each task lands

Do not run multiple implementer subagents in parallel for the same plan — superpowers' guidance is fresh subagent per task, in series.

---

## Output Files

| File | Purpose |
|------|---------|
| `docs/prd.md` | Product requirements with analytics framework (spec input to superpowers) |
| `docs/tasks.md` | Single source of truth for milestones and work items |
| `docs/planning.md` | Architecture, tech stack, integrations, dev workflow |
| `docs/checkpoint.md` | Progress tracking, updated each sprint and before commits |
| `CLAUDE.md` | (Project root) AI dev instructions, MCP tools, design standards, superpowers dev pattern |
| `docs/data-schema.md` | Database/analytics schema |
| `docs/dashboard-spec.md` | Dashboard wireframe specs |
| `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md` | Design spec (written in Phase 1 by `superpowers:brainstorming`, if the idea needed clarifying) |
| `docs/superpowers/plans/YYYY-MM-DD-<feature>.md` | First sprint's execution plan (written in Phase 3 by `superpowers:writing-plans` at the end of this run); later sprints add more |

## Reference Files

| Reference | When to Read |
|-----------|-------------|
| `references/prd-template.md` | Step 2 — PRD structure |
| `references/analytics-framework.md` | Step 3 — data spine and metrics |
| `references/clinic-mode.md` | Step 4 — healthcare requirements |
| `references/ad-platform-apis.md` | Step 3 — ad platform integration specs |
| `references/responsive-ui.md` | Step 2/Design — hooks, patterns, budgets |
| `references/testing-workflow.md` | Quality Gates — Playwright CLI, sprint docs |
| `references/planning-template.md` | Step 8 — `docs/planning.md` structure |
| `references/checkpoint-template.md` | Step 9 — `docs/checkpoint.md` structure |
| `references/claude-md-template.md` | Step 10 — CLAUDE.md template |
| `references/tasks-template.md` | Step 7 — task list guidelines |
| `references/mcp-configuration.md` | MCP setup guide |
