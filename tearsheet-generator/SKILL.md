---
name: tearsheet-generator
description: |
  Generate branded Triptyq Capital company tearsheets — concise one-page company profiles for pipeline review and LP presentations. Use this skill when someone needs a tearsheet, one-pager, company snapshot, deal card, or quick company profile. Triggers on "tearsheet for [company]", "one-pager on [company]", "company snapshot", "deal card", "quick profile of [company]", "make a tearsheet", or when a company summary is needed for a deck, LP meeting, or pipeline review. Lighter than an investment memo — this is the business card of a deal.
---

# Triptyq Capital — Tearsheet Generator

## What Is a Tearsheet

A single-page branded document that gives anyone the full picture of a company in 60 seconds. Used for pipeline reviews, LP meetings, conference prep, and sharing deals with co-investors.

## Research (5 Minutes)

1. **Web search**: company website, Crunchbase/PitchBook, recent news
2. **Affinity/email**: any prior relationship or interaction
3. **SharePoint**: check if deal folder exists with more data

## Tearsheet Layout (Single Page)

Build with docx-js. Triptyq brand tokens (RED C8102E, Georgia body, Arial headers, justified). Logo top-left.

```
[TRIPTYQ LOGO]                              [Date]

═══════════════════════════════════════════
[COMPANY NAME]
[Tagline / one-line description]
═══════════════════════════════════════════

SNAPSHOT                         ROUND
─────────                        ─────
Sector: [vertical]               Stage: [seed/A/B]
Founded: [year]                  Raising: [$amount]
HQ: [city, country]              Valuation: [$pre-money]
Employees: [count]               Lead: [investor]
Website: [url]                   Close: [target date]

WHAT THEY DO
[3-4 sentences. Product, market, differentiation. Dense.]

TEAM
[Founder 1] — [credential]
[Founder 2] — [credential]
[Key hire if notable]

TRACTION
[2-3 bullet points: revenue, users, partnerships, milestones]

THESIS FIT
[2 sentences: how this connects to entertainment × spatial × AI]

TRIPTYQ EDGE
[1-2 sentences: relationship, access, or strategic value we bring]

KEY RISK
[1 sentence: the single biggest concern]

─────────────────────────────────────────
VERDICT: [SCREENING / DILIGENCE / IC-READY / INVESTED / PASS]
─────────────────────────────────────────
CONFIDENTIAL — Triptyq Capital
```

## Design Rules

- ONE PAGE MAXIMUM. If it doesn't fit on one page, you're not distilling enough.
- Two-column layout for Snapshot + Round (use table with no borders)
- Company name in large bold black, tagline in red italic
- Red top and bottom rules
- Verdict bar at the bottom with status

## Batch Mode

When generating tearsheets for multiple companies (e.g., for an LP meeting or pipeline review), run them in parallel and compile into a single multi-page document with page breaks between each.

## Filing

Save to `03_Occasions Invest/02_Deal Flow/[###]_[Company]/From Triptyq/` if a deal folder exists. For batch tearsheets or LP presentations, save to `02b_Fund II (2025)/02_FINANCIAL/02_DECK/`.
