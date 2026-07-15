# Clinic Mode Specification

## Overview

CLINIC MODE enables specialized features and requirements for healthcare/clinic clients. This mode adds patient journey tracking, healthcare-specific metrics, compliance requirements, and vertical-specific configurations.

**Enable CLINIC MODE when:**
- Client operates a medical clinic or healthcare practice
- App handles patient information
- Services include consultations, treatments, or health programs
- Revenue comes from healthcare services

---

## Supported Healthcare Verticals

### Primary Verticals

| Vertical | Key Services | Revenue Range | Typical Program Length |
|----------|--------------|---------------|----------------------|
| **GLP-1 / Weight Loss** | Semaglutide, Tirzepatide programs | $2K-$15K | 3-12 months |
| **Women's Hormone** | Menopause, HRT, fertility | $3K-$12K | 6-24 months |
| **Men's TRT** | Testosterone, vitality | $2K-$8K | Ongoing |
| **Aesthetic Medspa** | Botox, fillers, lasers | $500-$10K | Per treatment |
| **Metabolic/Longevity** | Diabetes reversal, longevity | $5K-$20K | 6-12 months |

### Vertical-Specific Tracking

**GLP-1 / Weight Loss:**
```
Milestones to track:
- Initial weigh-in
- Weekly check-ins (weight, measurements)
- 10lb lost milestone
- 25lb lost milestone  
- Goal weight reached
- 3-month maintenance
- 6-month maintenance

KPIs:
- Avg weight loss (lbs)
- Program completion rate
- Medication adherence rate
- Side effect reports
```

**Women's Hormone:**
```
Milestones to track:
- Symptom assessment baseline
- Lab results received
- Protocol started
- 30-day symptom check
- 90-day optimization
- Annual review

KPIs:
- Symptom improvement score
- Treatment adherence
- Lab value improvements
- Quality of life score
```

**Men's TRT:**
```
Milestones to track:
- Initial labs
- Protocol started
- 6-week labs
- Dose optimization
- Quarterly check-ins

KPIs:
- Testosterone levels (before/after)
- Energy/vitality score
- Treatment adherence
- Renewal rate
```

---

## Patient Journey Framework

### Standard Clinic Funnel

```
AWARENESS → LEAD → CONSULTATION → PROGRAM → MEMBER → ADVOCATE
    │         │         │            │         │         │
    ▼         ▼         ▼            ▼         ▼         ▼
  Ad/Content  Form    Booking     Purchase   Recurring  Referral
  Impression  Submit   Completed   Complete   Payment    Made
```

### Extended Patient Touchpoints

```
PRE-CARE:
├── Lead captured
├── Automated nurture sequence
├── Lead contacted (response time tracked)
├── Consultation booked
├── Pre-consult education delivered
└── Consultation attended

IN-CARE:
├── Program purchased (tier tracked)
├── Onboarding completed
├── Weekly/monthly check-ins
├── Milestone achievements
├── Progress tracked (weight, labs, symptoms)
└── Education content consumed

POST-CARE:
├── Program completed
├── Review requested
├── Referral prompted
├── Membership conversion
└── Maintenance/renewal
```

### Patient Engagement Events

```javascript
// Program Milestone
{
  event: "milestone_achieved",
  properties: {
    patient_id: "string",
    program_id: "string",
    milestone_type: "weight_loss|lab_improvement|symptom_reduction|completion",
    milestone_name: "string",
    milestone_value: "number|string",
    days_in_program: "number"
  }
}

// Check-in Completed
{
  event: "checkin_completed",
  properties: {
    patient_id: "string",
    checkin_type: "weight|symptom|satisfaction|photo",
    checkin_data: {}, // Type-specific data
    streak_days: "number"
  }
}

// Education Content Consumed
{
  event: "education_consumed",
  properties: {
    patient_id: "string",
    content_id: "string",
    content_type: "video|article|pdf|quiz",
    completion_percent: "number",
    time_spent_seconds: "number"
  }
}
```

---

## Healthcare-Specific Metrics

### Patient Acquisition Metrics

| Metric | Definition | Benchmark | Alert |
|--------|------------|-----------|-------|
| **Patient Acquisition Cost (PAC)** | Marketing spend / new patients | $150-$500 | >$750 |
| **Cost per Lead (CPL)** | Ad spend / leads | $15-$50 | >$100 |
| **Lead-to-Patient Rate** | Patients / leads | 15-30% | <10% |
| **Consultation Show Rate** | Attended / booked | 70-85% | <60% |

### Patient Lifetime Value (PLV)

```
PLV CALCULATION:

Simple: Average Revenue per Patient × Average Patient Tenure

Detailed:
PLV = (Avg Initial Purchase) 
    + (Avg Monthly Membership × Avg Tenure Months)
    + (Avg Upsells per Year × Avg Tenure Years)
    - (Avg Churn Cost)

Example (GLP-1 Clinic):
- Initial Program: $4,000
- Monthly Membership: $299 × 18 months = $5,382
- Annual Upsells: $500 × 1.5 years = $750
- PLV = $10,132
```

### Program Metrics

| Metric | Definition | Target |
|--------|------------|--------|
| **Program Completion Rate** | % completing full program | >70% |
| **Milestone Achievement Rate** | % hitting key milestones | >60% |
| **Medication Adherence** | % following protocol | >85% |
| **Check-in Compliance** | % completing scheduled check-ins | >75% |

### Retention Metrics

| Metric | Definition | Target |
|--------|------------|--------|
| **Membership Conversion** | % programs converting to membership | >40% |
| **Monthly Membership Churn** | % members lost per month | <5% |
| **Annual Retention Rate** | % retained after 12 months | >60% |
| **Referral Rate** | % patients making referrals | >15% |

---

## Program Tier Structure

### Standard Tiers

| Tier | Price Range | Typical Inclusions | Margin |
|------|-------------|-------------------|--------|
| **Core** | $2,000 - $5,000 | Basic program, limited support | 50-60% |
| **Premium** | $5,000 - $15,000 | Full program, coaching, app access | 60-70% |
| **Elite** | $15,000+ | VIP access, 1:1 support, concierge | 70-80% |

### Tier Tracking Requirements

```javascript
{
  event: "program_enrolled",
  properties: {
    patient_id: "string",
    program_id: "string",
    program_name: "string",
    tier: "core|premium|elite",
    revenue: "number",
    duration_weeks: "number",
    includes_membership: "boolean",
    upsold_from: "string|null",  // If upgrade from lower tier
    attribution_source: "string"
  }
}
```

---

## Compliance Requirements

### HIPAA Considerations

**Protected Health Information (PHI) includes:**
- Name, address, phone, email
- Medical record numbers
- Lab results, diagnoses
- Treatment information
- Photos showing patient identity

**PRD Requirements when handling PHI:**

1. **Data Storage**
   - PHI must be stored in HIPAA-compliant infrastructure
   - Encryption at rest and in transit required
   - Access logging mandatory

2. **Data Transmission**
   - All API calls containing PHI must use HTTPS
   - PHI should not be included in analytics events sent to third-party platforms
   - Hash or anonymize identifiers before sending to analytics

3. **Business Associate Agreements (BAA)**
   - Required for any vendor handling PHI
   - Document BAA status for: hosting, analytics, communication tools

### Consent Requirements

```
REQUIRED CONSENTS:

1. SMS/Text Messaging
   - Explicit opt-in required
   - TCPA compliance language
   - Easy opt-out mechanism

2. Email Communications
   - CAN-SPAM compliance
   - Unsubscribe in every email
   - Physical address included

3. App Usage
   - Terms of service acceptance
   - Privacy policy acknowledgment
   - Data usage consent

4. Marketing Communications
   - Separate from transactional consent
   - Clear description of message types
   - Frequency disclosure
```

### Content Disclaimers

Include in PRD for healthcare apps:

```
REQUIRED DISCLAIMERS:

1. Medical Advice Disclaimer:
"This app provides educational information only and is not a substitute 
for professional medical advice, diagnosis, or treatment."

2. Results Disclaimer:
"Individual results may vary. The testimonials and examples used are 
not intended to represent or guarantee results."

3. Emergency Disclaimer:
"If you are experiencing a medical emergency, call 911 or go to the 
nearest emergency room immediately."
```

---

## Clinic Dashboard Specifications

### Clinic Scorecard (Monthly)

```
┌────────────────────────────────────────────────────────────────────┐
│  MONTHLY CLINIC SCORECARD                          Month: [Date]   │
├────────────────────────────────────────────────────────────────────┤
│                                                                    │
│  ACQUISITION                        │  CONVERSION                  │
│  ─────────────                      │  ──────────                  │
│  New Leads:        XXX (+XX%)       │  Lead→Consult:    XX%       │
│  Lead Sources:                      │  Show Rate:       XX%       │
│    Meta Ads:       XX%              │  Consult→Sale:    XX%       │
│    Google:         XX%              │  Avg Days to Sale: XX       │
│    Organic:        XX%              │                              │
│  Cost per Lead:    $XX              │                              │
│  Response Time:    X.X hrs          │                              │
│                                                                    │
├────────────────────────────────────────────────────────────────────┤
│                                                                    │
│  REVENUE                            │  RETENTION                   │
│  ───────                            │  ─────────                   │
│  Total Revenue:    $XX,XXX          │  Membership Churn:  X.X%    │
│    Baseline:       $XX,XXX          │  Active Members:    XXX     │
│    Incremental:    $XX,XXX          │  Repeat Visits:     XX%     │
│  By Tier:                           │  NPS Score:         XX      │
│    Core:           $XX,XXX (XX%)    │  Reviews (month):   XX      │
│    Premium:        $XX,XXX (XX%)    │  Referrals:         XX      │
│    Elite:          $XX,XXX (XX%)    │                              │
│                                                                    │
├────────────────────────────────────────────────────────────────────┤
│  KEY WINS THIS MONTH                │  FOCUS AREAS NEXT MONTH     │
│  ──────────────────                 │  ────────────────────       │
│  • [Win 1]                          │  • [Area 1]                 │
│  • [Win 2]                          │  • [Area 2]                 │
│  • [Win 3]                          │  • [Area 3]                 │
└────────────────────────────────────────────────────────────────────┘
```

### Quarterly Business Review (QBR) Dashboard

Include:
1. 90-day trend analysis (all metrics)
2. Goal vs actual comparison
3. Cohort analysis (patients by start month)
4. Revenue attribution breakdown
5. Experiment results summary
6. Next quarter recommendations

---

## Integration Points (Clinic-Specific)

### EHR/EMR Integration

If connecting to practice management systems:

| System | Integration Type | Data Sync |
|--------|-----------------|-----------|
| Jane App | API | Appointments, patients |
| Practice Better | Webhook | Forms, scheduling |
| Cerbo | API | Patient records |
| Custom EHR | CSV Export | Manual sync |

### Communication Platforms

| Platform | Use Case | Compliance |
|----------|----------|------------|
| GoHighLevel | Lead nurture, SMS | BAA available |
| Twilio | SMS/Voice | HIPAA eligible |
| Violet AI | Conversational SMS | BAA required |

### Scheduling Systems

| System | Features |
|--------|----------|
| Calendly | Self-booking, reminders |
| Acuity | Intake forms, payments |
| GHL Calendar | Native integration |

---

## Clinic Mode Checklist

### PRD Must Include:

- [ ] Vertical-specific milestone tracking
- [ ] Program tier definitions with pricing
- [ ] PAC and PLV calculations
- [ ] HIPAA compliance requirements (if PHI handled)
- [ ] Consent management specifications
- [ ] Required disclaimers
- [ ] Monthly scorecard design
- [ ] QBR dashboard requirements
- [ ] Integration points for clinic systems
