# Analytics Framework Specification

## Overview

This framework ensures every app can demonstrate measurable business impact through comprehensive data collection, attribution tracking, and visualization.

## The Data Spine

The Data Spine is a standardized data architecture that every app must implement. It provides consistent metrics across all clients and enables proof-of-concept demonstrations.

### Four Pillars of the Data Spine

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                            DATA SPINE ARCHITECTURE                          │
├───────────────────┬──────────────────┬─────────────────┬───────────────────┤
│    ACQUISITION    │    CONVERSION    │     REVENUE     │     RETENTION     │
├───────────────────┼──────────────────┼─────────────────┼───────────────────┤
│ Leads by source   │ Lead → Consult   │ Baseline rev    │ Membership churn  │
│ Cost per lead     │ Consult show %   │ Incremental rev │ Repeat frequency  │
│ Response time     │ Consult → Sale   │ Revenue by tier │ NPS score         │
│ Channel source    │ Program → Member │ Rev by source   │ Review volume     │
└───────────────────┴──────────────────┴─────────────────┴───────────────────┘
```

---

## Pillar 1: Acquisition Metrics

### Required Data Points

| Metric | Definition | Calculation | Storage |
|--------|------------|-------------|---------|
| **Total Leads** | Count of new contacts | COUNT(leads) | Daily aggregate |
| **Leads by Source** | Breakdown by acquisition channel | GROUP BY source | Daily aggregate |
| **Cost per Lead (CPL)** | Ad spend / leads | SUM(spend) / COUNT(leads) | Daily by source |
| **Lead Response Time** | Time from capture to first contact | AVG(first_contact_time - lead_time) | Per lead |

### Source Taxonomy

Standardized source values for consistent attribution:

```
PRIMARY SOURCES:
├── paid
│   ├── meta_ads          (Facebook, Instagram)
│   ├── google_ads        (Search, Display, YouTube)
│   ├── tiktok_ads
│   ├── reddit_ads
│   └── other_paid
├── organic
│   ├── organic_search
│   ├── organic_social
│   ├── direct
│   └── referral
├── offline
│   ├── walk_in
│   ├── phone_call
│   ├── event
│   └── flyer_qr
└── partner
    ├── affiliate
    ├── street_partner
    └── fleet_downstream
```

### UTM Parameter Standards

All marketing links must use these UTM parameters:

```
Required:
- utm_source: Platform (meta, google, tiktok, reddit, email, etc.)
- utm_medium: Type (cpc, social, email, referral, etc.)
- utm_campaign: Campaign name (slug format)

Recommended:
- utm_content: Ad/creative identifier
- utm_term: Keyword (for search)
- [custom]_ref: Internal reference ID
```

---

## Pillar 2: Conversion Metrics

### Funnel Stages

Standard conversion funnel with tracking points:

```
LEAD → QUALIFIED → CONSULTATION_BOOKED → CONSULTATION_ATTENDED → PURCHASED → MEMBER
  │        │              │                      │                   │          │
  ▼        ▼              ▼                      ▼                   ▼          ▼
Event   Event          Event                  Event               Event      Event
```

### Required Conversion Rates

| Metric | Definition | Target Range | Alert Threshold |
|--------|------------|--------------|-----------------|
| **Lead → Consult Rate** | % leads booking consultation | 15-30% | <10% |
| **Consult Show Rate** | % booked that attend | 70-85% | <60% |
| **Consult → Sale Rate** | % attended that purchase | 30-50% | <20% |
| **Sale → Member Rate** | % purchases becoming members | 40-60% | <30% |

### No-Show Tracking

Track consultation outcomes for rescue automation:

```javascript
{
  event: "consultation_outcome",
  properties: {
    consultation_id: "string",
    status: "completed|no_show|rescheduled|cancelled",
    no_show_reason: "string|null",
    rescue_attempted: "boolean",
    rescue_result: "rescheduled|lost|pending"
  }
}
```

---

## Pillar 3: Revenue Metrics

### Revenue Classification

All revenue must be classified as:

```
REVENUE TYPES:
├── baseline_revenue      (Existing before our involvement)
├── incremental_revenue   (Directly attributed to our channels)
│   ├── first_purchase    (New customer first transaction)
│   └── upsell            (Existing customer new product)
└── membership_revenue    (Recurring/subscription)
    ├── new_membership
    └── renewal
```

### Revenue Tiers (For Clinic Mode)

| Tier | Price Range | Description | Tracking Priority |
|------|-------------|-------------|-------------------|
| **Core** | $2,000 - $5,000 | Entry programs | High |
| **Premium** | $5,000 - $15,000 | Comprehensive programs | High |
| **Elite** | $15,000+ | VIP/intensive programs | Highest |

### Revenue Attribution

Track revenue back to original lead source:

```javascript
{
  event: "revenue_recorded",
  properties: {
    transaction_id: "string",
    customer_id: "string",
    amount: "number",
    currency: "USD",
    revenue_type: "baseline|incremental|membership",
    product_tier: "core|premium|elite|other",
    attribution: {
      first_touch_source: "string",
      last_touch_source: "string",
      campaign_id: "string",
      days_to_conversion: "number"
    }
  }
}
```

---

## Pillar 4: Retention Metrics

### Core Retention KPIs

| Metric | Definition | Good | Excellent |
|--------|------------|------|-----------|
| **Monthly Churn** | % members lost per month | <5% | <3% |
| **Repeat Visit Rate** | % returning within 90 days | >40% | >60% |
| **NPS Score** | Net Promoter Score | >30 | >50 |
| **Review Rate** | % customers leaving reviews | >10% | >25% |

### Engagement Scoring

Track user engagement to predict retention:

```
ENGAGEMENT SCORE = weighted sum of:
├── App opens (last 7 days)           × 1
├── Content consumed (last 30 days)   × 2
├── Appointments kept (last 90 days)  × 5
├── Milestones completed (all time)   × 3
└── Referrals made (all time)         × 10
```

### Review & Referral Triggers

Prompt reviews at "win moments":

```
TRIGGER CONDITIONS:
1. Milestone completed (e.g., 10lb lost, 30 days in program)
2. Positive check-in logged
3. Goal achieved
4. Anniversary date (30, 60, 90 days)

PROMPT SEQUENCE:
1. In-app celebration → 
2. "Share your success?" → 
3. Review link (Google/Facebook) → 
4. Referral code display
```

---

## Ad Platform Data Integration

### Required Metrics by Platform

**Meta Ads (Facebook/Instagram):**
```
Account level: account_id, account_name, currency
Campaign level: campaign_id, campaign_name, objective, status
Ad Set level: adset_id, adset_name, targeting, budget
Ad level: ad_id, ad_name, creative_id
Metrics: impressions, clicks, spend, conversions, cpc, cpm, ctr, roas
```

**Google Ads:**
```
Account level: customer_id, descriptive_name, currency
Campaign level: campaign_id, name, status, type
Ad Group level: ad_group_id, name, status
Ad level: ad_id, type, final_urls
Metrics: impressions, clicks, cost, conversions, avg_cpc, ctr, conv_rate
```

**TikTok Ads:**
```
Account level: advertiser_id, advertiser_name
Campaign level: campaign_id, campaign_name, objective_type
Ad Group level: adgroup_id, adgroup_name
Ad level: ad_id, ad_name
Metrics: impressions, clicks, spend, conversions, cpc, cpm, ctr, vtr
```

**Reddit Ads:**
```
Account level: account_id
Campaign level: campaign_id, name, objective
Ad Group level: ad_group_id, name
Ad level: ad_id, name
Metrics: impressions, clicks, spend, conversions, ecpc, ecpm, ctr
```

### Data Sync Schedule

| Platform | Pull Frequency | Latency | Historical Data |
|----------|---------------|---------|-----------------|
| Meta Ads | Daily 6am | 24-48hr | 37 months |
| Google Ads | Daily 6am | ~24hr | 3 years |
| TikTok Ads | Daily 6am | 24-48hr | 2 years |
| Reddit Ads | Daily 6am | ~24hr | 1 year |

---

## Dashboard Specifications

### Required Dashboard Views

**1. Executive Dashboard**
```
┌──────────────────────────────────────────────────────────────┐
│  HEADLINE METRICS (Period: Last 30 Days)                     │
│  ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────┐ │
│  │ Revenue    │ │ Leads      │ │ Conv Rate  │ │ CAC        │ │
│  │ $XX,XXX    │ │ XXX        │ │ XX%        │ │ $XXX       │ │
│  │ +XX% ▲     │ │ +XX% ▲     │ │ +X% ▲      │ │ -X% ▼      │ │
│  └────────────┘ └────────────┘ └────────────┘ └────────────┘ │
├──────────────────────────────────────────────────────────────┤
│  REVENUE BY SOURCE              │  FUNNEL CONVERSION         │
│  ┌─────────────────────────┐    │  ┌─────────────────────┐   │
│  │ ████████████ Meta 45%   │    │  │ Leads      │ 100%   │   │
│  │ ████████ Google 30%     │    │  │ Qualified  │ 65%    │   │
│  │ ████ Organic 15%        │    │  │ Consults   │ 42%    │   │
│  │ ██ Other 10%            │    │  │ Sales      │ 28%    │   │
│  └─────────────────────────┘    │  └─────────────────────┘   │
└──────────────────────────────────────────────────────────────┘
```

**2. Acquisition Dashboard**
- Lead volume over time (line chart)
- Leads by source (pie/bar chart)
- Cost per lead by source (bar chart)
- Lead response time distribution (histogram)
- Campaign performance table

**3. Conversion Dashboard**
- Funnel visualization (sankey or bar)
- Conversion rates by stage (gauge charts)
- Consultation outcomes (donut chart)
- Revenue by product tier (stacked bar)
- Attribution path analysis

**4. Retention Dashboard**
- Churn rate trend (line chart)
- Cohort retention (heatmap)
- NPS distribution (histogram)
- Review volume and rating trend
- Engagement score distribution

---

## Implementation Checklist

### Minimum Viable Analytics (Week 1-2)
- [ ] Event tracking schema defined
- [ ] Core events implemented (lead, consult, purchase)
- [ ] Source attribution tracking live
- [ ] Basic dashboard with headline metrics

### Full Analytics (Week 3-4)
- [ ] All funnel events tracked
- [ ] Ad platform APIs connected
- [ ] Revenue attribution complete
- [ ] Full dashboard suite deployed

### Advanced Analytics (Week 5+)
- [ ] Multi-touch attribution model
- [ ] Predictive churn scoring
- [ ] A/B test infrastructure
- [ ] Automated reporting/alerts
