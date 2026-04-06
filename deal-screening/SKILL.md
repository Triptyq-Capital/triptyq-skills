---
name: deal-screening
description: |
  Quick first-look screening of a deal for Triptyq Capital's weekly pipeline review. Use this skill for initial deal assessment before committing to full due diligence. Triggers on "screen this deal", "first look at [company]", "should we take a meeting", "quick assessment", "pipeline review", "does this fit our thesis", "is this worth a deeper look", or when a new company enters the pipeline and needs a go/no-go for the next partners meeting. Lighter than vc-due-diligence — this is the 15-minute filter, not the 2-hour deep dive.
---

# Triptyq Capital — Deal Screening

## Purpose

The weekly Dealflow & Portcos meeting needs a fast, structured way to assess new deals. This skill produces a 1-page screening memo that answers one question: should we spend time on this?

## The Three Gates

Every deal must pass three gates to proceed. If it fails any gate, it's a pass (with a note on why).

### Gate 1: Thesis Fit
Does this connect to entertainment × spatial computing × AI?
- Direct fit: the company builds in one of these verticals
- Adjacent fit: infrastructure that enables these verticals (data, tools, platforms)
- No fit: interesting but outside our mandate

### Gate 2: Team Signal
Do the founders have an unfair advantage?
- Prior exits (especially to FAANG or strategic acquirers)
- Deep domain expertise (not just "worked at Google for 2 years")
- Relationship to Triptyq network (Denis, Jaron, portfolio founders, LPs)
- Repeat founders vs first-time

### Gate 3: Deal Economics
Is the math reasonable?
- Stage vs valuation (seed at $20M = fine, seed at $500M = needs exceptional justification)
- Round structure (lead investor quality, allocation available)
- Geography (North America preferred, Singapore/international = extra diligence)

## Output Format (1 Page)

```
DEAL SCREENING: [Company Name]
Date: [today]  |  Source: [who referred]  |  Screened by: [partner]

THESIS FIT:     [DIRECT / ADJACENT / NO FIT]
TEAM SIGNAL:    [STRONG / MODERATE / WEAK]
DEAL ECONOMICS: [ATTRACTIVE / FAIR / STRETCH]

VERDICT: [TAKE THE MEETING / WATCH / PASS]

What they do: [2 sentences max]
Why now: [1 sentence — market timing or catalyst]
Team: [founder names + most impressive credential each, 1 line per founder]
Round: [stage, amount, valuation, lead if known]
Thesis connection: [1-2 sentences linking to entertainment × spatial × AI]
Key question: [the single most important thing to validate in a first meeting]
Red flag: [if any — jurisdiction, conflict, valuation, missing info]
```

## Research (Keep it Fast)

This is a 15-minute exercise, not a deep dive:
1. **Web search**: company name + founders (2-3 searches max)
2. **Affinity/email**: have we seen this company before?
3. **SharePoint**: check if a deal folder already exists
4. **Crunchbase/PitchBook**: funding history if available

Do NOT read legal docs, pull SEC filings, or analyze cap tables at this stage. That's for the vc-due-diligence skill after the screening passes.

## After Screening

- If TAKE THE MEETING: create the deal folder on SharePoint (`[###]_[Company]/From Co/ + From Triptyq/`), save screening doc to `From Triptyq/`
- If WATCH: note in the partners meeting agenda, revisit in 2-4 weeks
- If PASS: record reason in Affinity, no folder needed

## Partners Meeting Integration

The screening output is designed to be read aloud in 60 seconds at the weekly meeting. Charles or whoever is presenting can scan it and hit the key points without reading a full memo.
