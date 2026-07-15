# planning.md Template

## Overview

`docs/planning.md` is the strategic foundation document that captures vision, architecture, technology decisions, tooling requirements, and the superpowers-based development workflow. It serves as the source of truth for technical direction.

---

## Template

```markdown
# planning.md - [Project Name]

**Version:** 1.0  
**Last Updated:** [Date]  
**Authors:** [Names]

---

## 1. Vision Statement

### What We're Building
[2-3 sentences describing the product and its core purpose]

### Core Value Proposition
[What unique value does this deliver? Why will users choose this?]

### Success Looks Like
| Timeframe | Success Metric | Target |
|-----------|---------------|--------|
| 30 days | [Metric] | [Value] |
| 60 days | [Metric] | [Value] |
| 90 days | [Metric] | [Value] |

### Key Differentiators
1. [What makes this different from alternatives?]
2. [Unique capability or approach]
3. [Competitive advantage]

---

## 2. Architecture Overview

### System Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                        SYSTEM ARCHITECTURE                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────┐     ┌──────────────┐     ┌──────────────┐     │
│  │   Client     │     │   API        │     │   Database   │     │
│  │   (Web/App)  │────▶│   Server     │────▶│   Layer      │     │
│  └──────────────┘     └──────────────┘     └──────────────┘     │
│         │                    │                    │              │
│         │                    ▼                    │              │
│         │             ┌──────────────┐            │              │
│         │             │   External   │            │              │
│         └────────────▶│   Services   │◀───────────┘              │
│                       └──────────────┘                           │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Component Breakdown

| Component | Responsibility | Technology |
|-----------|---------------|------------|
| Frontend | User interface, client-side logic | [Tech] |
| API Layer | Business logic, data processing | [Tech] |
| Database | Data persistence, querying | [Tech] |
| Cache | Performance, session management | [Tech] |
| Queue | Async processing, jobs | [Tech] |
| Analytics | Event tracking, reporting | [Tech] |

### Data Flow

```
User Action → Frontend Event → API Request → Business Logic 
    → Database Write → Analytics Event → Response → UI Update
```

### Key Architectural Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| API Style | REST / GraphQL | [Why this choice] |
| Database Type | SQL / NoSQL | [Why this choice] |
| Hosting Model | Serverless / Traditional | [Why this choice] |
| Auth Strategy | JWT / Sessions | [Why this choice] |

---

## 3. Technology Stack

### Frontend

| Layer | Technology | Version | Purpose |
|-------|-----------|---------|---------|
| Framework | [React/Vue/Next.js/etc.] | [X.X] | Core UI framework |
| State Management | [Redux/Zustand/etc.] | [X.X] | Application state |
| Styling | [Tailwind/styled-components/etc.] | [X.X] | CSS framework |
| UI Components | [shadcn/MUI/etc.] | [X.X] | Component library |
| Charts | [Recharts/Chart.js/etc.] | [X.X] | Data visualization |
| Forms | [React Hook Form/Formik/etc.] | [X.X] | Form handling |

### Backend

| Layer | Technology | Version | Purpose |
|-------|-----------|---------|---------|
| Runtime | [Node.js/Python/etc.] | [X.X] | Server runtime |
| Framework | [Express/FastAPI/etc.] | [X.X] | API framework |
| ORM | [Prisma/SQLAlchemy/etc.] | [X.X] | Database access |
| Validation | [Zod/Joi/etc.] | [X.X] | Input validation |
| Auth | [Passport/NextAuth/etc.] | [X.X] | Authentication |

### Database & Storage

| Type | Technology | Purpose |
|------|-----------|---------|
| Primary DB | [PostgreSQL/MySQL/etc.] | Main data store |
| Cache | [Redis/Memcached] | Performance cache |
| File Storage | [S3/Cloudflare R2/etc.] | Media/documents |
| Search | [Elasticsearch/Algolia] | Full-text search (if needed) |

### Infrastructure

| Service | Provider | Purpose |
|---------|----------|---------|
| Hosting | [Vercel/AWS/Railway/etc.] | Application hosting |
| Database Hosting | [Supabase/PlanetScale/etc.] | Managed database |
| CDN | [Cloudflare/Fastly/etc.] | Content delivery |
| DNS | [Cloudflare/Route53/etc.] | Domain management |

---

## 4. Required Tools

### Development Tools

| Tool | Purpose | Required? |
|------|---------|-----------|
| Git | Version control | ✅ Yes |
| VS Code / Cursor | IDE | ✅ Yes |
| Docker | Containerization | ⚡ Recommended |
| Postman / Insomnia | API testing | ✅ Yes |
| pgAdmin / TablePlus | Database management | ✅ Yes |

### Deployment & CI/CD

| Tool | Purpose | Required? |
|------|---------|-----------|
| GitHub Actions / GitLab CI | CI/CD pipeline | ✅ Yes |
| Vercel / Netlify | Frontend deployment | ✅ Yes |
| Railway / Render | Backend deployment | ✅ Yes |
| Terraform / Pulumi | Infrastructure as code | ⚡ Recommended |

### Monitoring & Observability

| Tool | Purpose | Required? |
|------|---------|-----------|
| Sentry | Error tracking | ✅ Yes |
| LogTail / Datadog | Log management | ✅ Yes |
| Uptime Robot / Better Uptime | Uptime monitoring | ✅ Yes |
| Grafana / Datadog | Metrics dashboard | ⚡ Recommended |

### Analytics & Data

| Tool | Purpose | Required? |
|------|---------|-----------|
| Segment / RudderStack | Event routing | ⚡ Recommended |
| Mixpanel / Amplitude | Product analytics | ✅ Yes |
| Metabase / Looker | BI dashboard | ✅ Yes |
| dbt | Data transformation | ⚡ Optional |

### Communication & Project Management

| Tool | Purpose | Required? |
|------|---------|-----------|
| Slack / Discord | Team communication | ✅ Yes |
| Linear / Jira | Project tracking | ✅ Yes |
| Notion / Confluence | Documentation | ✅ Yes |
| Figma | Design collaboration | ⚡ Recommended |

### MCP Tools (Claude AI Development)

| Tool | Purpose | When to Use |
|------|---------|-------------|
| **Playwright MCP** | Testing | All E2E, integration, and component tests |
| **Context7 MCP** | Documentation & Implementation | Implementation clarity, code patterns, library usage |
| **shadcn/ui MCP** | UI Components | Building/modifying UI with ShadCN components |

> ⚠️ **Important:** Document these MCP tools in the project's `Claude.md` file for consistent AI-assisted development.

---

## 5. Integration Points

### External APIs

| API | Purpose | Auth Method | Rate Limits |
|-----|---------|-------------|-------------|
| Meta Marketing API | Ad performance data | OAuth 2.0 | 200/hr/account |
| Google Ads API | Campaign metrics | OAuth 2.0 | 10K ops/day |
| TikTok Marketing API | Video ad data | OAuth 2.0 | 600/min |
| Reddit Ads API | Conversion tracking | API Token | 100/min |
| [CRM API] | Customer data | API Key | [Limit] |
| [Payment API] | Transactions | API Key | [Limit] |

### Webhooks (Inbound)

| Source | Event Types | Endpoint |
|--------|-------------|----------|
| [Service] | [Events] | `/webhooks/[service]` |
| Stripe | payment_intent.succeeded, etc. | `/webhooks/stripe` |
| GoHighLevel | contact.created, etc. | `/webhooks/ghl` |

### Webhooks (Outbound)

| Destination | Trigger | Payload |
|-------------|---------|---------|
| [Service] | [Event] | [Data sent] |
| Slack | New lead | Lead details |
| CRM | Purchase | Customer + transaction |

### Data Import/Export

| Direction | Format | Frequency | Method |
|-----------|--------|-----------|--------|
| Import | CSV | Manual | File upload |
| Import | API | Daily | Scheduled job |
| Export | CSV | On-demand | Download |
| Export | API | Real-time | Webhook |

---

## 6. Security & Compliance

### Authentication & Authorization

| Aspect | Implementation |
|--------|---------------|
| Auth Method | [JWT / Session / OAuth] |
| Password Policy | Min 8 chars, 1 uppercase, 1 number |
| MFA | [Optional / Required / N/A] |
| Session Duration | [X hours/days] |
| Role-Based Access | [Roles defined] |

### Data Protection

| Data Type | Protection Method |
|-----------|------------------|
| Passwords | bcrypt (cost factor 12) |
| PII | AES-256 encryption at rest |
| API Keys | Encrypted, not logged |
| Tokens | Short-lived, secure storage |

### Compliance Requirements

| Standard | Applicable? | Implementation |
|----------|------------|----------------|
| GDPR | [Yes/No] | [How addressed] |
| CCPA | [Yes/No] | [How addressed] |
| HIPAA | [Yes/No] | [How addressed] |
| SOC 2 | [Yes/No] | [How addressed] |
| PCI-DSS | [Yes/No] | [How addressed] |

### HIPAA Considerations (Clinic Mode)

If handling PHI:
- [ ] BAAs with all vendors handling PHI
- [ ] Encryption at rest and in transit
- [ ] Access logging and audit trails
- [ ] Minimum necessary data access
- [ ] Breach notification procedures
- [ ] Employee training documentation

---

## 7. Development Workflow (Superpowers)

This project uses the `superpowers` skill family as the spine of its development cycle. Every new piece of work flows through the same 6 steps:

1. **Read the 4 source-of-truth docs first**:
   - `docs/prd.md` — what we're building
   - `docs/tasks.md` — what's left and what's in progress
   - `docs/planning.md` — this file (architecture, tech stack, this workflow)
   - `docs/checkpoint.md` — current status, blockers, recent changes
2. **Plan the next stage** — invoke `superpowers:writing-plans` against the next unstarted P0/P1 task in `docs/tasks.md`. Superpowers writes the plan to `docs/plans/YYYY-MM-DD-<feature>.md`.
3. **Execute** — invoke `superpowers:executing-plans` (or `superpowers:subagent-driven-development` for multi-task plans).
4. **TDD** — every plan follows Red → Verify Red → Green → Verify Green → Refactor via `superpowers:test-driven-development`. No production code without a failing test first.
5. **Verify** — `superpowers:verification-before-completion` gates every "done" claim. Fresh test/build evidence required.
6. **Track** — edit `docs/tasks.md` (check off items, add discovered tasks) and `docs/checkpoint.md` (status, blockers, change log) as work lands. Do not use `TaskCreate`/`TaskUpdate`.

### Why this matters

Without this pattern, every new session starts cold and either skips the spec or invents its own plan. With it, any session reads the 4 docs, sees exactly where the project is, and produces structured, tested, verified work.

---

## 8. Scalability Considerations

### Current Capacity Targets

| Metric | Initial Target | Growth Target |
|--------|---------------|---------------|
| Concurrent Users | [X] | [X] |
| Requests/Second | [X] | [X] |
| Data Storage | [X GB] | [X GB] |
| Monthly Events | [X] | [X] |

### Scaling Strategy

| Component | Scaling Approach |
|-----------|-----------------|
| Frontend | CDN, edge caching |
| API | Horizontal (auto-scale) |
| Database | Read replicas, connection pooling |
| Background Jobs | Queue workers (auto-scale) |

### Known Bottlenecks

| Bottleneck | Threshold | Mitigation |
|------------|-----------|------------|
| Database connections | [X] concurrent | Connection pooling |
| API rate limits (external) | [X]/hour | Caching, queuing |
| File uploads | [X] MB | Pre-signed URLs, CDN |

### Future Architecture Considerations

- [ ] Microservices split (when to consider)
- [ ] Multi-region deployment
- [ ] Real-time features (WebSockets)
- [ ] Mobile app API optimization

---

## Appendix

### Environment Variables

```
# Application
NODE_ENV=development|staging|production
API_URL=
APP_URL=

# Database
DATABASE_URL=
REDIS_URL=

# Authentication
JWT_SECRET=
SESSION_SECRET=

# External APIs
META_APP_ID=
META_APP_SECRET=
GOOGLE_ADS_DEVELOPER_TOKEN=
TIKTOK_APP_ID=
TIKTOK_APP_SECRET=

# Services
SENTRY_DSN=
STRIPE_SECRET_KEY=
```

### Glossary

| Term | Definition |
|------|------------|
| [Term] | [Definition] |

### References

- [Link to design files]
- [Link to API documentation]
- [Link to related PRD]
```

---

## Guidelines for planning.md

### When to Update

- Initial project setup
- Major architectural changes
- New technology additions
- Compliance requirement changes
- Scale threshold updates

### Best Practices

1. **Keep it Current:** Outdated architecture docs are worse than none
2. **Be Specific:** Include versions, not just technology names
3. **Document Decisions:** Capture the "why" not just the "what"
4. **Include Diagrams:** Visual architecture is easier to understand
5. **Link to Details:** Reference detailed docs rather than duplicating
