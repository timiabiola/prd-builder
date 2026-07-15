# PRD Builder

A Claude Code skill that orchestrates a full, superpowers-integrated project bootstrap — turning a rough idea into a complete Product Requirements Document, an analytics/measurement framework, and a ready-to-build project scaffold in one continuous run.

## What it does

When you say *"I want to build X"* (or even just *"help me figure out what to build"*), prd-builder drives the whole arc:

1. **Brainstorm** (when the idea isn't clear) — explores intent, proposes approaches, and writes an approved design spec.
2. **PRD + scaffold** — generates `docs/prd.md`, `docs/tasks.md`, `docs/planning.md`, `docs/checkpoint.md`, and a root `CLAUDE.md`, with KPIs, attribution, and a "proof dashboard" baked in from day one.
3. **First sprint plan** — optionally writes the first sprint's execution plan, grounded in both the spec and the PRD.

Every app it bootstraps is forced to answer one question up front: **"How do we prove this is working?"** — conversion funnels, ad-platform attribution, and success metrics are first-class citizens, not afterthoughts.

Includes a **CLINIC MODE** for healthcare clients (HIPAA, patient journey tracking, program tiers, BAA considerations).

## Why it's especially valuable for non-technical founders/operators

Most PRD tools assume you already speak engineer. This one doesn't.

- **You bring the business knowledge; the skill brings the structure.** You describe the problem, the customers, and the revenue model in plain language. The skill translates that into a prioritized feature list (P0/P1/P2), a technical architecture, a data schema, and a task roadmap — the artifacts an engineer (or an AI coding agent) can build directly from.
- **No blank-page paralysis.** Vague ideas are run through a guided brainstorm first, so you never have to know the "right" answer before you start. The skill asks the decision questions (web or mobile? subscription or one-time? HIPAA?) and proposes sensible defaults.
- **A shared language with your builder.** The four output docs are the single source of truth. Hand them to a developer, an agency, or Claude Code itself, and everyone is looking at the same picture — milestones, success metrics, what's done, what's next. No more "what did we agree on again?"
- **Proof, not vibes.** Because analytics and attribution are designed in alongside features, you can demonstrate ROI from launch instead of bolting on tracking later and guessing what worked.
- **A workflow that survives the next session.** The scaffold wires in a repeatable 6-step development pattern (read the docs → plan → execute → test → verify → update docs), so the project never loses its place — even if weeks pass between sessions.

In short: it turns "I have an idea and I'm not technical" into "here is a buildable plan, and here is how we'll know it worked."

## Install

Copy the `prd-builder/` folder into your Claude Code skills directory (e.g. `~/.claude/skills/prd-builder/`), then invoke it with `/prd-builder` or by describing a new project ("plan this app", "create a PRD", "bootstrap this project").

## Credits & dependencies

This skill stands on the shoulders of two excellent open-source projects. It orchestrates their skills/tools directly — without them, prd-builder is just templates.

- **[Superpowers](https://github.com/obra/superpowers)** — created by **[Jesse Vincent (obra)](https://github.com/obra)**. An agentic skills framework and software development methodology. prd-builder uses Superpowers' `brainstorming`, `writing-plans`, `executing-plans`, `subagent-driven-development`, `test-driven-development`, and `verification-before-completion` skills as the spine of its workflow.

- **[Context7](https://github.com/upstash/context7)** — created by **[Upstash](https://github.com/upstash)** (Vsevolod Romashov / [@upsuper](https://github.com/upsuper)). Up-to-date, version-specific documentation for LLMs and AI code editors. prd-builder uses Context7 to pull current library docs during planning and debugging so the scaffolded projects start on accurate, non-hallucinated API patterns.

Full credit for the methodology and tooling below belongs to their respective authors; this skill is a glue layer that composes them into a single bootstrap flow.

## License

MIT
