---
name: sharepoint-filing
description: |
  Navigate and file documents in Triptyq Capital's SharePoint structure. Use this skill whenever someone needs to save a document to SharePoint, find where a file should go, create a new deal folder, organize due diligence materials, or file investment memos, legal docs, meeting notes, or any work product in the right place. Triggers on "save to SharePoint", "file this", "put this in the deal folder", "where does this go", "create a folder for [company]", "organize these docs", or any reference to filing, archiving, or storing documents in the Triptyq shared drive. Also use when the investment-memo or vc-due-diligence skills produce output that needs to be filed.
---

# Triptyq Capital SharePoint Filing

## Site Info
- **Site**: Triptyq Capital — `https://triptyq.sharepoint.com/sites/TriptyqCapital/`
- **Library**: Documents Triptyq

## Top-Level Structure

```
Documents Triptyq/
├── 01_Gestion Triptyq/          ← Fund ops, legal, HR, branding
├── 02b_Fund II (2025)/          ← Fund II: legal, deck, dataroom
├── 03_Occasions Invest/         ← ALL deal activity
│   ├── 01_Portco/               ← Active portfolio companies
│   ├── 02_Deal Flow/            ← Pipeline deals (active + passed)
│   ├── 03_Partners Meeting/     ← Weekly meeting notes
│   └── Other/                   ← PitchBook, market research
```

## Deal Flow Convention

Every deal gets a numbered folder under `03_Occasions Invest/02_Deal Flow/`:

```
[###]_[Company Name]/
├── From Co/              ← Docs FROM the company (legal, cap tables, decks, data room)
├── From Triptyq/         ← Docs WE produce (memos, DD reports, analysis)
└── Internal/ (optional)  ← Deeper analysis
    ├── 01_Preliminary Analysis/
    └── 03_Deliverables/
        └── 01_Investment Memo/
```

### Naming: `[###]_[Company Name]` — sequential numbers, common name (not legal entity).
Examples: `110_Calder AI`, `100_VVD`, `74_QARL.AI`, `59_Anomaly`

## Filing Quick Reference

| Document Type | Destination Path |
|--------------|-----------------|
| Investment memo | `02_Deal Flow/[###]_[Co]/From Triptyq/` |
| DD report | `02_Deal Flow/[###]_[Co]/From Triptyq/` |
| Legal docs from lawyers | `02_Deal Flow/[###]_[Co]/From Co/` |
| Cap table / pro forma | `02_Deal Flow/[###]_[Co]/From Co/` |
| Company pitch deck | `02_Deal Flow/[###]_[Co]/From Co/` |
| Data room exports | `02_Deal Flow/[###]_[Co]/From Co/` |
| Market research | `Other/Pitchbook/Market/` |
| Meeting notes | `03_Partners Meeting/[Year]/[Month FR]/[YYYY-MM-DD]/` |
| Fund deck | `02b_Fund II (2025)/02_FINANCIAL/02_DECK/` |
| Logos | `01_Gestion Triptyq/01_LEGAL_CORPO/08_Trademark, Logo, Domains/` |
| PitchBook fund research | `Other/Pitchbook/Funds/` |

Month names are French: Fév, Mars, Avril, Mai, Juin, Juillet, Août, Sept, Oct, Nov, Déc.

## Portfolio Companies

Once invested, companies move to `01_Portco/[##]_[Company]/` with financial subfolders per round.

## Partners Meeting

```
03_Partners Meeting (weekly)/[Year]/[Month FR]/[YYYY-MM-DD]/
```
Files: `Compte-rendu_partners_meeting.docx`, `PARTNERS MEETING [DATE].docx`, pipeline xlsx.

## PitchBook Reports

```
Other/Pitchbook/
├── Triptyq/   ← About our fund
├── Market/    ← Sector research
└── Funds/     ← Competitor funds
```

## How to File

1. **Determine type** using the table above
2. **Search for existing folder**: `sharepoint_folder_search(name="[Company]")`
3. **If no folder exists**: find the highest number in Deal Flow, create next sequential `[###]_[Company]/` with `From Co/` and `From Triptyq/` subfolders
4. **Upload or provide path** for the user to file manually

## Creating a New Deal Folder

1. Search existing deal flow folders for the next available number
2. Create `03_Occasions Invest/02_Deal Flow/[###]_[Company Name]/`
3. Create `From Co/` and `From Triptyq/`
4. If diligence is starting, also create `Internal/01_Preliminary Analysis/`

## Rules

- **From Co vs From Triptyq**: Never mix. Company docs in `From Co/`, our work in `From Triptyq/`.
- **Numbering**: Always sequential, always check what exists first.
- **French months**: The meeting folders use French month names. Use them.
- **Don't restructure**: Follow existing conventions even if inconsistent. The team knows where things are.
- **File immediately**: When the investment-memo or vc-due-diligence skill produces output, offer to file it before ending.
