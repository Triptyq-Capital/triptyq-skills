---
name: partners-meeting-prep
description: |
  Prepare the weekly Triptyq Capital partners meeting (Dealflow & Portcos Weekly). Use this skill to generate the agenda, pipeline update, portfolio status, and compte-rendu template. Triggers on "prep the partners meeting", "weekly meeting agenda", "dealflow update", "portco weekly", "Sunday meeting prep", "prepare for our weekly", "meeting prep for Charles/Guillaume/Bert", or any reference to the recurring Dealflow & Portcos Weekly meeting. Also use for generating the compte-rendu (meeting minutes) after the meeting.
---

# Triptyq Capital — Partners Meeting Prep

## Meeting Context

The Dealflow & Portcos Weekly is the core operating rhythm of Triptyq Capital. Held every Sunday/Monday with Charles, Guillaume, Bert, and Elisabeth. The meeting covers new deal flow, portfolio updates, CRM hygiene, and action items.

## Before the Meeting

### Data Sources to Pull
1. **Affinity CRM**: Deals updated in the last 7 days, new companies added, status changes
2. **Email (Outlook)**: Search for founder emails, term sheets, investor updates received this week
3. **Granola**: Pull notes from last week's meeting for follow-up tracking
4. **SharePoint**: Check `03_Partners Meeting/[Year]/[Month FR]/` for last week's compte-rendu and tracker
5. **Calendar**: Upcoming meetings with founders, LPs, conferences

### Generate Agenda

```
DEALFLOW & PORTCOS WEEKLY
[Date] | [Time]
Participants: Charles, Guillaume, Bert, Elisabeth

1. OPEN MIC (5 min)
   - Quick wins, blockers, announcements

2. NEW DEAL FLOW ([count] new this week)
   [For each new company, run the deal-screening skill output:]
   - [Company] | [Source] | [Verdict: MEET/WATCH/PASS]

3. PIPELINE STATUS UPDATES
   [For each deal with activity this week:]
   - [Company] | [Status change] | [Next step] | [Owner]

4. PORTFOLIO UPDATES
   [For each portco with news:]
   - [Company] | [Update] | [Action needed?]

5. CRM & PROCESS
   - Affinity hygiene: [count] companies missing data
   - Follow-ups overdue: [list]

6. ACTION ITEMS FROM LAST WEEK
   [Pull from previous compte-rendu]
   - [Item] | [Owner] | [Status: DONE/IN PROGRESS/OVERDUE]

7. NEXT WEEK PREVIEW
   - Key meetings scheduled
   - Deadlines approaching
```

## After the Meeting (Compte-Rendu)

Generate meeting minutes in French (the team operates bilingually but internal notes are typically in French).

### File Naming Convention
- `Compte-rendu_reunion_[date].docx` or `Compte-rendu_partners_meeting.docx`
- Pipeline tracker: `Triptyq_Partners_Meeting_[Date].xlsx`

### Save Location
`03_Occasions Invest/03_Partners Meeting (weekly)/[Year]/[Month FR]/[YYYY-MM-DD]/`

Month names in French: Janvier, Février, Mars, Avril, Mai, Juin, Juillet, Août, Septembre, Octobre, Novembre, Décembre.

## Integration with Other Skills

- New deals → run **deal-screening** for each
- Deal progressing to diligence → trigger **vc-due-diligence**
- Deal ready for IC → trigger **investment-memo**
- All outputs → file via **sharepoint-filing**
