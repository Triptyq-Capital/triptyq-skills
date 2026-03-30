# Triptyq Skills

Shared Claude skills for Triptyq Capital.

## Skills

### vc-due-diligence

Run comprehensive due diligence on any company. Dynamically discovers and chains all available data sources on Rube.app — SEC EDGAR, Crunchbase, Apollo.io, OpenCorporates, Composio search tools, and any future additions.

**Triggers:** "due diligence on [company]", "DD on [company]", "research [company] for investment", "pull everything on [company]"

**Output:** Inline summary + .docx report with funding history, SEC filings, team/leadership, competitive landscape, corporate registry data, and risk factors.

**Setup:** Copy `vc-due-diligence/` into your Claude Team's shared `.claude/skills/` directory.

## Adding New Skills

Each skill lives in its own directory with a `SKILL.md` file and optional `evals/` folder for test cases.
