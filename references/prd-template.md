# PRD Template

## Document Header

```markdown
# Product Requirements Document
**Project Name:** [Name]
**Version:** 1.0
**Date:** [Date]
**Author:** [Name]
**Status:** Draft | Review | Approved

## Change Log
| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0     | [Date] | [Name] | Initial draft |
```

---

## 1. Executive Summary

### 1.1 Problem Statement
[2-3 sentences describing the problem this product solves]

### 1.2 Proposed Solution
[2-3 sentences describing the solution]

### 1.3 Success Metrics (Preview)
| Metric | Baseline | Target | Timeframe |
|--------|----------|--------|-----------|
| [Primary KPI] | [Current] | [Goal] | [Days/Months] |
| [Secondary KPI] | [Current] | [Goal] | [Days/Months] |

### 1.4 Mode
- [ ] **STANDARD MODE** - General business application
- [ ] **CLINIC MODE** - Healthcare/clinic application (enables additional requirements)

---

## 2. User Personas & Journeys

### 2.1 Primary Persona
**Name:** [Persona Name]
**Role:** [Job title/role]
**Goals:**
- [Goal 1]
- [Goal 2]

**Pain Points:**
- [Pain point 1]
- [Pain point 2]

### 2.2 User Journey Map

```
[Awareness] → [Consideration] → [Conversion] → [Retention] → [Advocacy]
     ↓              ↓               ↓              ↓            ↓
  [Touchpoint]  [Touchpoint]   [Touchpoint]   [Touchpoint]  [Touchpoint]
```

**Journey Stages with Tracking Events:**

| Stage | User Action | Tracking Event | Data Captured |
|-------|-------------|----------------|---------------|
| Awareness | Sees ad/content | `page_view`, `ad_impression` | Source, campaign, device |
| Consideration | Downloads resource | `lead_magnet_download` | Email, source, content_id |
| Conversion | Books consultation | `consultation_booked` | DateTime, service_type |
| Purchase | Completes purchase | `purchase_complete` | Revenue, product_tier |
| Retention | Returns for service | `repeat_visit` | Visit count, LTV |
| Advocacy | Refers friend | `referral_sent` | Referral_code, referred_email |

---

## 3. Core Features

### 3.1 Feature Priority Matrix

| Priority | Feature | User Value | Business Value | Complexity |
|----------|---------|------------|----------------|------------|
| P0 | [Feature] | High | High | Low |
| P0 | [Feature] | High | High | Medium |
| P1 | [Feature] | Medium | High | Medium |
| P2 | [Feature] | Medium | Medium | High |

### 3.2 Feature Specifications

#### Feature 1: [Name] (P0)
**Description:** [What it does]
**User Story:** As a [user type], I want to [action] so that [benefit].
**Acceptance Criteria:**
- [ ] [Criterion 1]
- [ ] [Criterion 2]

**Analytics Events:**
- Event: `[event_name]`
- Properties: `{ property1, property2, ... }`
- Trigger: [When this event fires]

---

## 4. Analytics & Data Requirements (MANDATORY)

### 4.1 Data Spine Architecture

Every feature must connect to this core data framework:

```
┌─────────────────────────────────────────────────────────────────┐
│                        DATA SPINE                                │
├─────────────────────────────────────────────────────────────────┤
│  ACQUISITION          CONVERSION           REVENUE    RETENTION │
│  ─────────────        ──────────           ───────    ───────── │
│  • Leads by source    • Lead→Consult       • Baseline • Churn   │
│  • Cost per lead      • Consult show rate  • Incremental        │
│  • Response time      • Consult→Sale       • By tier  • NPS     │
│  • Channel attribution• By program type    • By source• Reviews │
└─────────────────────────────────────────────────────────────────┘
```

### 4.2 Required Tracking Events

**Acquisition Events:**
```javascript
// Lead Captured
{
  event: "lead_captured",
  properties: {
    source: "meta_ads|google_ads|tiktok|reddit|organic|referral|other",
    campaign_id: "string",
    utm_source: "string",
    utm_medium: "string",
    utm_campaign: "string",
    utm_content: "string",
    landing_page: "string",
    timestamp: "ISO8601"
  }
}

// Lead Response
{
  event: "lead_contacted",
  properties: {
    lead_id: "string",
    response_time_seconds: "number",
    contact_method: "sms|email|phone|chat",
    contacted_by: "automated|human"
  }
}
```

**Conversion Events:**
```javascript
// Consultation Booked
{
  event: "consultation_booked",
  properties: {
    lead_id: "string",
    consultation_type: "string",
    scheduled_datetime: "ISO8601",
    booking_source: "self_serve|assisted"
  }
}

// Consultation Completed
{
  event: "consultation_completed",
  properties: {
    consultation_id: "string",
    outcome: "purchased|declined|followup_needed",
    duration_minutes: "number",
    service_discussed: "string"
  }
}

// Purchase Complete
{
  event: "purchase_complete",
  properties: {
    customer_id: "string",
    transaction_id: "string",
    revenue: "number",
    product_tier: "core|premium|elite",
    product_name: "string",
    attribution_source: "string",
    is_incremental: "boolean"
  }
}
```

**Retention Events:**
```javascript
// Membership Created
{
  event: "membership_created",
  properties: {
    customer_id: "string",
    membership_tier: "string",
    monthly_value: "number",
    start_date: "ISO8601"
  }
}

// Membership Churned
{
  event: "membership_churned",
  properties: {
    customer_id: "string",
    membership_id: "string",
    churn_reason: "string",
    lifetime_value: "number",
    tenure_months: "number"
  }
}

// Review Submitted
{
  event: "review_submitted",
  properties: {
    customer_id: "string",
    platform: "google|facebook|yelp|internal",
    rating: "1-5",
    has_text: "boolean"
  }
}
```

### 4.3 Ad Platform Integration Requirements

See `references/ad-platform-apis.md` for detailed specifications.

| Platform | Required Data | Sync Frequency | Authentication |
|----------|---------------|----------------|----------------|
| Meta Ads | Campaigns, ad sets, ads, insights | Daily | OAuth 2.0 |
| Google Ads | Campaigns, keywords, conversions | Daily | OAuth 2.0 |
| TikTok Ads | Campaigns, creatives, metrics | Daily | OAuth 2.0 |
| Reddit Ads | Campaigns, conversions | Daily | API Token |

### 4.4 Attribution Model

**Default Model:** Last-touch with 30-day lookback
**Recommended Model:** Linear multi-touch with position-based weighting

```
Attribution Windows:
- Click-through: 7 days (configurable to 1, 7, 28 days)
- View-through: 1 day (configurable to 1, 7 days)

Position Weights (Multi-touch):
- First touch: 40%
- Middle touches: 20% (split equally)
- Last touch: 40%
```

---

## 5. Technical Architecture

### 5.1 System Overview

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Client    │────▶│   API/BFF   │────▶│  Database   │
│  (App/Web)  │     │   Layer     │     │  (Events)   │
└─────────────┘     └─────────────┘     └─────────────┘
                           │                    │
                           ▼                    ▼
                    ┌─────────────┐     ┌─────────────┐
                    │ Ad Platform │     │  Analytics  │
                    │    APIs     │     │  Dashboard  │
                    └─────────────┘     └─────────────┘
```

### 5.2 Data Schema

**Core Tables:**
- `leads` - Lead information and source tracking
- `consultations` - Booking and outcome data
- `transactions` - Revenue and purchase data
- `memberships` - Recurring revenue tracking
- `events` - Raw event log for analytics

### 5.3 Integration Points

| System | Integration Type | Data Flow | Frequency |
|--------|-----------------|-----------|-----------|
| GoHighLevel | Webhook/API | Bidirectional | Real-time |
| Meta Ads | API Pull | Inbound | Daily |
| Google Ads | API Pull | Inbound | Daily |
| Analytics DB | Event Stream | Outbound | Real-time |

---

## 6. Success Metrics & KPIs

### 6.1 Primary KPIs (Headline Metrics)

| KPI | Definition | Baseline | Target | Measurement |
|-----|------------|----------|--------|-------------|
| Lead Volume | Total leads captured | [X/month] | [+Y%] | Daily |
| Lead-to-Consult Rate | Leads that book consultation | [X%] | [Y%] | Weekly |
| Revenue (Incremental) | New revenue from our channels | $0 | $[X] | Monthly |
| Customer Acquisition Cost | Total spend / new customers | N/A | <$[X] | Monthly |

### 6.2 Secondary KPIs

| KPI | Definition | Target |
|-----|------------|--------|
| Lead Response Time | Avg time to first contact | <[X] hours |
| Consultation Show Rate | % of booked that attend | >[X]% |
| Average Order Value | Revenue per transaction | $[X] |
| Customer Lifetime Value | Total revenue per customer | $[X] |
| Net Promoter Score | Customer satisfaction | >[X] |

### 6.3 Dashboard Requirements

**Required Dashboard Views:**

1. **Executive Summary** (C-level)
   - Total revenue (incremental vs baseline)
   - Lead volume trend
   - ROI by channel
   - Key conversion rates

2. **Acquisition Dashboard** (Marketing)
   - Leads by source
   - Cost per lead by channel
   - Campaign performance
   - Ad spend vs revenue

3. **Operations Dashboard** (Team)
   - Lead response times
   - Consultation schedule
   - Pipeline value
   - Task completion

---

## 7. Timeline & Milestones

### 7.1 Phase 1: Foundation (Weeks 1-2)
- [ ] Data schema design
- [ ] Core tracking implementation
- [ ] Basic dashboard setup

### 7.2 Phase 2: Integration (Weeks 3-4)
- [ ] Ad platform connections
- [ ] Event tracking live
- [ ] Attribution model active

### 7.3 Phase 3: Optimization (Weeks 5-8)
- [ ] Full dashboard deployment
- [ ] A/B testing infrastructure
- [ ] Automated reporting

---

## 8. Risks & Mitigations

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Data quality issues | Medium | High | Implement validation at capture |
| API rate limits | Low | Medium | Implement queuing and caching |
| Attribution accuracy | Medium | High | Use server-side tracking + pixel |
| Privacy compliance | Low | High | Follow platform guidelines, hash PII |

---

## Appendix A: Glossary

| Term | Definition |
|------|------------|
| Incremental Revenue | Revenue directly attributable to our marketing efforts |
| Baseline | Performance metrics before implementation |
| CAC | Customer Acquisition Cost |
| PLV/LTV | Patient/Customer Lifetime Value |
| ROAS | Return on Ad Spend |

---

## Appendix B: How This PRD Is Used

This document is one of four source-of-truth artifacts (alongside `docs/tasks.md`, `docs/planning.md`, `docs/checkpoint.md`). When development on the next stage begins:

1. The model reads all four docs to understand current state.
2. It invokes `superpowers:writing-plans` against the next unstarted P0/P1 work — superpowers creates and owns the resulting plan in `docs/plans/YYYY-MM-DD-<feature>.md`.
3. Execution uses `superpowers:executing-plans` or `superpowers:subagent-driven-development`, with TDD discipline (`superpowers:test-driven-development`) and verification gates (`superpowers:verification-before-completion`).
4. `docs/tasks.md` (check off items, add discovered tasks) and `docs/checkpoint.md` (status, blockers, change log) are updated as work lands.

This PRD is not a one-shot document. Update it whenever scope or features change, and the next planning cycle will pick up the changes automatically.
