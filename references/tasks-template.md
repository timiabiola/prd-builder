# tasks.md Template

## Overview

`docs/tasks.md` is the single source of truth for milestones and work items. Created at project kickoff and edited in-file as work progresses. No native task list, no `TaskCreate`/`TaskUpdate`, no `~/.claude/tasks/...` plumbing — just a markdown file in the repo.

---

## Template

```markdown
# tasks.md - [Project Name]

> [One-sentence project description]

**Created:** [Date]  
**Target Completion:** [Date]  
**Total Estimated Hours:** [X hours]

**Status legend:** `[ ]` not started · `[~]` in progress · `[x]` complete · `[!]` blocked

---

## Milestone 1: Foundation
**Timeline:** Week 1-2  
**Estimated Hours:** [X hours]  
**Definition of Done:** Core infrastructure operational, data schema implemented, dev environment ready

### Setup & Infrastructure
- [ ] Initialize repository and project structure
- [ ] Set up development environment (local + staging)
- [ ] Configure CI/CD pipeline
- [ ] Set up error tracking and logging
- [ ] Configure environment variables and secrets management

### Database & Schema
- [ ] Design data schema (see DATA-SCHEMA.md)
- [ ] Set up database (PostgreSQL/MySQL/etc.)
- [ ] Create migration scripts
- [ ] Implement seed data for development
- [ ] Set up database backup strategy

### Authentication & Security
- [ ] Implement authentication system
- [ ] Set up role-based access control
- [ ] Configure HTTPS/SSL
- [ ] Implement API rate limiting
- [ ] Set up security headers

**Dependencies:** None  
**Blockers to Watch:** Database hosting selection, auth provider decision

---

## Milestone 2: Core Features
**Timeline:** Week 3-5  
**Estimated Hours:** [X hours]  
**Definition of Done:** All P0 features functional, basic UI complete, API endpoints tested

### [Feature Category 1]
- [ ] [Task 1]
- [ ] [Task 2]
- [ ] [Task 3]

### [Feature Category 2]
- [ ] [Task 1]
- [ ] [Task 2]
- [ ] [Task 3]

### API Development
- [ ] Design API endpoints (REST/GraphQL)
- [ ] Implement CRUD operations for core entities
- [ ] Add input validation and error handling
- [ ] Write API documentation
- [ ] Create Postman/Insomnia collection

### UI/UX Implementation
- [ ] Build core layout/navigation
- [ ] Implement main user flows
- [ ] Add form validation and feedback
- [ ] Ensure mobile responsiveness
- [ ] Implement loading states and error handling

**Dependencies:** Milestone 1 complete  
**Blockers to Watch:** Design approval, API contract finalization

---

## Milestone 3: Analytics Integration
**Timeline:** Week 5-6  
**Estimated Hours:** [X hours]  
**Definition of Done:** All tracking events firing, ad platform data flowing, attribution working

### Event Tracking
- [ ] Define tracking event schema
- [ ] Implement core tracking events:
  - [ ] `lead_captured`
  - [ ] `consultation_booked`
  - [ ] `consultation_completed`
  - [ ] `purchase_complete`
  - [ ] `membership_created`
- [ ] Set up event validation
- [ ] Test event firing in all user flows

### Ad Platform Connections
- [ ] Meta Ads API integration
  - [ ] OAuth setup
  - [ ] Data pull implementation
  - [ ] Conversion tracking
- [ ] Google Ads API integration
  - [ ] Developer token setup
  - [ ] GAQL queries
  - [ ] Conversion import
- [ ] TikTok Ads API integration (if applicable)
- [ ] Reddit Ads API integration (if applicable)

### Attribution System
- [ ] Implement UTM parameter capture
- [ ] Set up first-touch/last-touch tracking
- [ ] Build attribution data model
- [ ] Create attribution reports

**Dependencies:** Milestone 2 complete, ad account access  
**Blockers to Watch:** Ad platform API approval, account permissions

---

## Milestone 4: Dashboard & Reporting
**Timeline:** Week 6-7  
**Estimated Hours:** [X hours]  
**Definition of Done:** All dashboards functional, data accurate, visualizations clear

### Executive Dashboard
- [ ] Headline metrics cards
- [ ] Revenue trend chart
- [ ] Funnel visualization
- [ ] Channel attribution breakdown

### Acquisition Dashboard
- [ ] Lead volume by source
- [ ] Cost per lead trends
- [ ] Campaign performance table
- [ ] Response time metrics

### Conversion Dashboard
- [ ] Funnel stage breakdown
- [ ] Conversion rate trends
- [ ] Consultation outcomes
- [ ] Revenue by tier

### Retention Dashboard (if applicable)
- [ ] Churn rate visualization
- [ ] Cohort retention heatmap
- [ ] NPS/review tracking
- [ ] Engagement metrics

### Reporting Features
- [ ] PDF/CSV export
- [ ] Scheduled email reports
- [ ] Date range filtering
- [ ] Comparison periods

**Dependencies:** Milestone 3 complete  
**Blockers to Watch:** Data quality, visualization library selection

---

## Milestone 5: Testing & QA
**Timeline:** Week 7-8  
**Estimated Hours:** [X hours]  
**Definition of Done:** All tests passing, edge cases handled, performance acceptable

### Automated Testing
- [ ] Unit tests for core functions (>80% coverage)
- [ ] Integration tests for API endpoints
- [ ] E2E tests for critical user flows
- [ ] Analytics event tests

### Manual Testing
- [ ] User acceptance testing (UAT)
- [ ] Cross-browser testing
- [ ] Mobile device testing
- [ ] Accessibility audit

### Performance Testing
- [ ] Load testing (target: X concurrent users)
- [ ] Database query optimization
- [ ] API response time (<200ms target)
- [ ] Frontend performance (Lighthouse >90)

### Security Testing
- [ ] Penetration testing / security scan
- [ ] Input validation review
- [ ] Authentication flow review
- [ ] Data encryption verification

**Dependencies:** Milestones 1-4 complete  
**Blockers to Watch:** Test environment stability, UAT scheduling

---

## Milestone 6: Launch & Optimization
**Timeline:** Week 8+  
**Estimated Hours:** [X hours]  
**Definition of Done:** Production deployed, monitoring active, handoff complete

### Deployment
- [ ] Production environment setup
- [ ] DNS configuration
- [ ] SSL certificate installation
- [ ] Database migration to production
- [ ] Environment variable configuration

### Monitoring & Alerting
- [ ] Set up uptime monitoring
- [ ] Configure error alerting
- [ ] Set up performance monitoring
- [ ] Create runbook for common issues

### Documentation & Handoff
- [ ] Technical documentation
- [ ] User guide / help docs
- [ ] Admin training materials
- [ ] API documentation finalization

### Post-Launch
- [ ] Monitor for issues (first 48 hours)
- [ ] Collect initial user feedback
- [ ] Address critical bugs
- [ ] Plan iteration cycle

**Dependencies:** All previous milestones, stakeholder sign-off  
**Blockers to Watch:** Production hosting, domain setup, stakeholder availability

---

## Backlog (Future Iterations)
Items not in current scope but planned for future:

- [ ] [Feature idea 1]
- [ ] [Feature idea 2]
- [ ] [Enhancement 1]
- [ ] [Integration 1]

---

## Notes

### Assumptions
- [List key assumptions made during planning]

### Risks
- [Risk 1]: Mitigation strategy
- [Risk 2]: Mitigation strategy

### Out of Scope
- [Item explicitly not included]
- [Item explicitly not included]

---

## Milestone Dependencies

```
Milestone 1: Foundation → no dependencies
Milestone 2: Core Features → depends on: Milestone 1
Milestone 3: Analytics → depends on: Milestone 2
Milestone 4: Dashboard → depends on: Milestone 3
Milestone 5: Testing → depends on: Milestones 1-4
Milestone 6: Launch → depends on: Milestone 5
```

## Coordination

`docs/tasks.md` is the only place tasks live. It's committed to the repo and edited in-file by whoever (or whichever Claude session) is working. Subagents update the file directly — no env vars, no shared state mechanism beyond the file itself.

When development on a feature begins, the model invokes `superpowers:writing-plans` against the relevant PRD section. The resulting plan in `docs/plans/YYYY-MM-DD-<feature>.md` becomes the per-feature execution detail; `tasks.md` stays the high-level roadmap.
```

---

## Guidelines for tasks.md

### Task Writing Best Practices

1. **Be Specific:** "Implement user login with email/password" not "Add login"
2. **Estimate Consistently:** Use hours, and track actual vs estimated
3. **Include Acceptance Criteria:** What does "done" look like?
4. **Note Dependencies:** What must be complete first?
5. **Flag Blockers Early:** Identify potential issues before they hit

### Milestone Sizing

| Milestone Type | Typical Duration | Task Count |
|----------------|-----------------|------------|
| Foundation | 1-2 weeks | 15-25 tasks |
| Core Features | 2-3 weeks | 25-40 tasks |
| Analytics | 1-2 weeks | 15-25 tasks |
| Dashboard | 1-2 weeks | 15-20 tasks |
| Testing | 1 week | 15-20 tasks |
| Launch | 1 week | 10-15 tasks |

### Status Indicators

Use these checkbox states:
- `[ ]` - Not started
- `[~]` - In progress (optional convention)
- `[x]` - Complete
- `[!]` - Blocked (optional convention)

### Updating tasks.md

- Check off tasks as completed (edit checkboxes in-file)
- Add new tasks as discovered
- Move tasks between milestones if scope shifts
- Update estimates based on actuals
- Reference in `docs/checkpoint.md` updates
