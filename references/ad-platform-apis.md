# Ad Platform API Integration Specifications

## Overview

This document specifies the integration requirements for major ad platforms. All apps must be designed to ingest data from these sources to enable ROI tracking and attribution.

---

## Meta Ads API (Facebook & Instagram)

### Authentication

```
Auth Type: OAuth 2.0
Required Permissions:
- ads_read
- read_insights
- business_management (for multiple accounts)

Token Refresh: Required every 60 days
```

### API Endpoints

**Account Level:**
```
GET /act_{ad_account_id}
Response: account_id, name, currency, timezone
```

**Campaign Level:**
```
GET /act_{ad_account_id}/campaigns
Fields: id, name, objective, status, daily_budget, lifetime_budget
```

**Ad Set Level:**
```
GET /{campaign_id}/adsets
Fields: id, name, targeting, optimization_goal, billing_event, bid_amount
```

**Ad Level:**
```
GET /{adset_id}/ads
Fields: id, name, status, creative_id, tracking_specs
```

**Insights (Metrics):**
```
GET /{object_id}/insights
Fields: impressions, clicks, spend, cpc, cpm, ctr, reach, frequency,
        actions, conversions, cost_per_action_type, purchase_roas

Time Ranges: date_preset or time_range (max 37 months)
Breakdowns: age, gender, country, region, device_platform, 
            publisher_platform, placement
```

### Key Metrics to Capture

| Metric | API Field | Description |
|--------|-----------|-------------|
| Impressions | `impressions` | Total ad views |
| Clicks | `clicks` | Total clicks |
| Spend | `spend` | Amount spent |
| CPC | `cpc` | Cost per click |
| CPM | `cpm` | Cost per 1000 impressions |
| CTR | `ctr` | Click-through rate |
| Conversions | `actions` | Action counts by type |
| ROAS | `purchase_roas` | Return on ad spend |
| Reach | `reach` | Unique users reached |
| Frequency | `frequency` | Avg times user saw ad |

### Conversion Tracking

**Meta Pixel Events:**
```javascript
// Standard Events
fbq('track', 'Lead');
fbq('track', 'Schedule');
fbq('track', 'Purchase', {value: 100.00, currency: 'USD'});

// Custom Events
fbq('trackCustom', 'ConsultationBooked', {
  consultation_type: 'initial',
  service: 'weight_loss'
});
```

**Conversions API (Server-Side):**
```
POST /act_{ad_account_id}/events
Required: event_name, event_time, user_data (hashed), custom_data
Recommended: event_source_url, event_id (for deduplication)
```

---

## Google Ads API

### Authentication

```
Auth Type: OAuth 2.0
Required Scopes: https://www.googleapis.com/auth/adwords
Developer Token: Required (apply via Google Ads)
```

### API Structure

Google Ads API uses Google Ads Query Language (GAQL):

```sql
-- Campaign Performance
SELECT
  campaign.id,
  campaign.name,
  campaign.status,
  metrics.impressions,
  metrics.clicks,
  metrics.cost_micros,
  metrics.conversions,
  metrics.average_cpc
FROM campaign
WHERE segments.date DURING LAST_30_DAYS

-- Ad Group Performance
SELECT
  ad_group.id,
  ad_group.name,
  metrics.impressions,
  metrics.clicks,
  metrics.conversions
FROM ad_group
WHERE campaign.id = {campaign_id}

-- Keyword Performance (Search)
SELECT
  keyword_view.resource_name,
  ad_group_criterion.keyword.text,
  ad_group_criterion.keyword.match_type,
  metrics.impressions,
  metrics.clicks,
  metrics.cost_micros
FROM keyword_view
```

### Key Metrics to Capture

| Metric | API Field | Description |
|--------|-----------|-------------|
| Impressions | `metrics.impressions` | Total ad views |
| Clicks | `metrics.clicks` | Total clicks |
| Cost | `metrics.cost_micros` | Spend in micros (÷1M for dollars) |
| Avg CPC | `metrics.average_cpc` | Average cost per click |
| CTR | `metrics.ctr` | Click-through rate |
| Conversions | `metrics.conversions` | Conversion count |
| Conv Rate | `metrics.conversions_from_interactions_rate` | % clicks converting |
| Conv Value | `metrics.conversions_value` | Total conversion value |
| Quality Score | `ad_group_criterion.quality_info.quality_score` | Keyword quality |

### Conversion Tracking

**Google Ads Conversion Tag:**
```javascript
gtag('event', 'conversion', {
  'send_to': 'AW-XXXXXXXXX/XXXXXXXXX',
  'value': 100.00,
  'currency': 'USD'
});
```

**Enhanced Conversions:**
```javascript
gtag('set', 'user_data', {
  'email': 'hashed_email',
  'phone_number': 'hashed_phone'
});
```

**Offline Conversion Import:**
```
POST /customers/{customer_id}/offlineUserDataJobs:create
Upload: gclid, conversion_action, conversion_date_time, conversion_value
```

---

## TikTok Marketing API

### Authentication

```
Auth Type: OAuth 2.0
Required Scopes: ad_account_management, campaign_management, 
                 creative_management, report_management

Access Level: Standard or Advanced (based on approval)
```

### API Endpoints

**Advertiser Info:**
```
GET /open_api/v1.3/oauth2/advertiser/get/
Response: advertiser_id, advertiser_name, status
```

**Campaign Data:**
```
GET /open_api/v1.3/campaign/get/
Fields: campaign_id, campaign_name, objective_type, budget, status
```

**Ad Group Data:**
```
GET /open_api/v1.3/adgroup/get/
Fields: adgroup_id, adgroup_name, placement_type, targeting, budget
```

**Ad Data:**
```
GET /open_api/v1.3/ad/get/
Fields: ad_id, ad_name, status, creative_id
```

**Reporting:**
```
POST /open_api/v1.3/report/integrated/get/
Dimensions: campaign_id, adgroup_id, ad_id, stat_time_day
Metrics: spend, impressions, clicks, conversions, cpc, cpm, ctr,
         video_views, video_watched_2s, video_watched_6s
```

### Key Metrics to Capture

| Metric | API Field | Description |
|--------|-----------|-------------|
| Impressions | `impressions` | Total ad views |
| Clicks | `clicks` | Total clicks |
| Spend | `spend` | Amount spent |
| CPC | `cpc` | Cost per click |
| CPM | `cpm` | Cost per 1000 impressions |
| CTR | `ctr` | Click-through rate |
| Conversions | `conversions` | Total conversions |
| VTR | `video_watched_6s_rate` | 6-second view rate |
| Reach | `reach` | Unique users reached |

### Conversion Tracking

**TikTok Pixel Events:**
```javascript
ttq.track('Lead', { content_name: 'consultation' });
ttq.track('SubmitForm', { content_name: 'booking' });
ttq.track('Purchase', { value: 100, currency: 'USD' });
```

**Events API (Server-Side):**
```
POST /open_api/v1.3/event/track/
Required: pixel_code, event, timestamp
User Data: email (hashed), phone (hashed), external_id
Custom Data: content_type, content_id, value, currency
```

---

## Reddit Ads API

### Authentication

```
Auth Type: OAuth 2.0
Required Scopes: ads_read, ads_conversion
Token: Access token + refresh token
```

### API Endpoints

**Account Info:**
```
GET /api/v3/me/accounts
Response: id, name, currency, timezone
```

**Campaign Data:**
```
GET /api/v3/accounts/{account_id}/campaigns
Fields: id, name, objective, status, budget
```

**Ad Group Data:**
```
GET /api/v3/accounts/{account_id}/adgroups
Fields: id, name, campaign_id, targeting, bid
```

**Ad Data:**
```
GET /api/v3/accounts/{account_id}/ads
Fields: id, name, ad_group_id, creative, status
```

**Reporting:**
```
GET /api/v3/accounts/{account_id}/reports
Metrics: impressions, clicks, spend, ecpc, ecpm, ctr,
         conversions, conversion_rate, cost_per_conversion
```

### Key Metrics to Capture

| Metric | API Field | Description |
|--------|-----------|-------------|
| Impressions | `impressions` | Total ad views |
| Clicks | `clicks` | Total clicks |
| Spend | `spend` | Amount spent |
| eCPC | `ecpc` | Effective cost per click |
| eCPM | `ecpm` | Effective cost per 1000 |
| CTR | `ctr` | Click-through rate |
| Conversions | `conversions` | Total conversions |
| Conv Rate | `conversion_rate` | % clicks converting |

### Conversion Tracking

**Reddit Pixel:**
```javascript
rdt('track', 'Lead');
rdt('track', 'SignUp');
rdt('track', 'Purchase', { value: 100, currency: 'USD' });
```

**Conversions API (Server-Side):**
```
POST /api/v3/conversions/events
Required: pixel_id, event_type, timestamp
Match Keys: email (SHA256), device_id, click_id
Event Data: value, currency, item_count
```

---

## Data Aggregation Architecture

### Recommended Data Flow

```
┌─────────────────────────────────────────────────────────────────────┐
│                        DATA AGGREGATION LAYER                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐                   │
│  │ Meta    │ │ Google  │ │ TikTok  │ │ Reddit  │                   │
│  │ Ads API │ │ Ads API │ │ Ads API │ │ Ads API │                   │
│  └────┬────┘ └────┬────┘ └────┬────┘ └────┬────┘                   │
│       │           │           │           │                         │
│       └───────────┴─────┬─────┴───────────┘                         │
│                         │                                           │
│                    ┌────▼────┐                                      │
│                    │ ETL /   │                                      │
│                    │ Sync    │                                      │
│                    │ Service │                                      │
│                    └────┬────┘                                      │
│                         │                                           │
│                    ┌────▼────────────────────────────┐              │
│                    │     UNIFIED DATA WAREHOUSE      │              │
│                    │                                  │              │
│                    │  ┌──────────────────────────┐   │              │
│                    │  │ campaigns                │   │              │
│                    │  │ ad_sets                  │   │              │
│                    │  │ ads                      │   │              │
│                    │  │ daily_metrics            │   │              │
│                    │  │ conversions              │   │              │
│                    │  └──────────────────────────┘   │              │
│                    └────┬───────────────────────────┘              │
│                         │                                           │
│                    ┌────▼────┐                                      │
│                    │Dashboard│                                      │
│                    │ Layer   │                                      │
│                    └─────────┘                                      │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Unified Metrics Schema

```sql
-- Daily Ad Performance Table
CREATE TABLE ad_performance_daily (
  id UUID PRIMARY KEY,
  date DATE NOT NULL,
  platform VARCHAR(20) NOT NULL,  -- meta, google, tiktok, reddit
  account_id VARCHAR(100),
  campaign_id VARCHAR(100),
  campaign_name VARCHAR(255),
  adset_id VARCHAR(100),
  adset_name VARCHAR(255),
  ad_id VARCHAR(100),
  ad_name VARCHAR(255),
  
  -- Universal Metrics (normalized)
  impressions INTEGER,
  clicks INTEGER,
  spend DECIMAL(10,2),
  conversions INTEGER,
  conversion_value DECIMAL(10,2),
  
  -- Calculated Metrics
  cpc DECIMAL(10,4),        -- spend / clicks
  cpm DECIMAL(10,4),        -- (spend / impressions) * 1000
  ctr DECIMAL(10,6),        -- clicks / impressions
  cvr DECIMAL(10,6),        -- conversions / clicks
  roas DECIMAL(10,4),       -- conversion_value / spend
  cpa DECIMAL(10,4),        -- spend / conversions
  
  -- Metadata
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Index for common queries
CREATE INDEX idx_perf_date_platform ON ad_performance_daily(date, platform);
CREATE INDEX idx_perf_campaign ON ad_performance_daily(campaign_id);
```

---

## Rate Limits & Best Practices

### Rate Limits by Platform

| Platform | Rate Limit | Reset Window |
|----------|-----------|--------------|
| Meta | 200 calls/hour/ad account | Rolling |
| Google | 10,000 operations/day | Midnight PST |
| TikTok | 600 calls/minute | Rolling |
| Reddit | 100 calls/minute | Rolling |

### Best Practices

1. **Batch Requests:** Combine multiple object requests where supported
2. **Caching:** Cache account/campaign structure (changes rarely)
3. **Incremental Sync:** Only fetch data since last sync
4. **Error Handling:** Implement exponential backoff for rate limits
5. **Deduplication:** Use event_id/transaction_id to prevent double-counting

### Sync Schedule Recommendation

```
Daily Sync Schedule:

06:00 UTC - Google Ads (most stable data)
06:30 UTC - Meta Ads (24-48hr attribution delay)
07:00 UTC - TikTok Ads
07:30 UTC - Reddit Ads

Lookback Window:
- Standard: 3 days (catch attribution updates)
- Monthly: Full month resync on 3rd of each month
```

---

## Implementation Checklist

### Per Platform

- [ ] OAuth credentials configured
- [ ] Token refresh automation
- [ ] Rate limiting implemented
- [ ] Error handling/alerting
- [ ] Data validation rules
- [ ] Schema mapping defined

### Data Pipeline

- [ ] ETL jobs scheduled
- [ ] Deduplication logic
- [ ] Historical backfill complete
- [ ] Monitoring/alerting
- [ ] Data quality checks
