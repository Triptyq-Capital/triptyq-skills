---
name: portfolio-monitoring
description: |
  Monitor Triptyq Capital portfolio companies and generate status dashboards. Use this skill to check on portco health, flag issues, track milestones, prep for board meetings, or generate a portfolio overview. Triggers on "how are our portcos doing", "portfolio update", "check on [company]", "board meeting prep for [company]", "portfolio dashboard", "portco status", "any red flags in the portfolio", "runway check", or when someone needs a consolidated view of portfolio company activity and health.
---

# Triptyq Capital — Portfolio Monitoring

## Portfolio Companies

Check `03_Occasions Invest/01_Portco/` on SharePoint for the current list. As of early 2026, the portfolio includes 14+ companies across Fund I.

## Monitoring Cadence

| Check | Frequency | Source |
|-------|-----------|--------|
| Email activity | Weekly | Outlook search per portco |
| News mentions | Weekly | Web search per portco |
| Financial updates | Monthly/Quarterly | SharePoint portco folders |
| Board meetings | Per schedule | Calendar + Granola |
| Runway / burn | Quarterly | Financial reports in SharePoint |
| Milestone tracking | Ongoing | Affinity + email |

## Single Company Check

When someone asks "how is [company] doing?":

1. **Email**: Last 30 days of emails mentioning the company
2. **Granola**: Recent meeting notes involving the company
3. **SharePoint**: Latest documents in their portco folder
4. **Web search**: Recent news, product launches, hiring activity
5. **Affinity**: CRM notes and interaction history

Output a 1-paragraph status with: last contact date, latest milestone, any concerns, next scheduled touchpoint.

## Portfolio Dashboard

When someone asks for the full portfolio overview:

### For Each Company
```
[Company Name] | [Status: GREEN/YELLOW/RED]
Last contact: [date]
Latest update: [1 sentence]
Key metric: [revenue, users, or milestone]
Next touchpoint: [board meeting, check-in, or deadline]
Flag: [any concern, or "none"]
```

### Traffic Light System
- **GREEN**: On track, regular communication, no concerns
- **YELLOW**: Behind on a milestone, communication gap >30 days, or minor concern
- **RED**: Runway <6 months, founder conflict, missed targets, or material risk

### Aggregate Metrics
- Total portfolio value (if available)
- Companies by status (GREEN/YELLOW/RED count)
- Average days since last contact
- Upcoming board meetings this month

## Board Meeting Prep

When prepping for a specific company's board meeting:

1. Pull all documents from their SharePoint folder
2. Search email for founder correspondence (last 90 days)
3. Check Granola for previous board meeting notes
4. Web search for competitive landscape changes
5. Generate a 1-page brief: company status, key metrics, discussion topics, questions to ask

## Filing

Save monitoring reports to the relevant portco folder on SharePoint: `03_Occasions Invest/01_Portco/[##]_[Company]/`
