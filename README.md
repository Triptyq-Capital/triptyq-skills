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

## Sanctum Bridge wiring

Skills that push artifacts to SharePoint use `_shared/sanctum-bridge.md`
rather than hardcoding the endpoint URL or auth headers. The bridge sits
at `https://bridge.nepveu.name` (CF Access service token + HMAC at the
origin); allowed top-level folder roots live in
`sharepoint-structure.yaml`.

To regenerate the auto-managed section of the helper after a bridge
deploy, run:

```sh
python3 scripts/sync-bridge-manifest.py
```

The script reads its credentials from env (`CF_ACCESS_ID`,
`CF_ACCESS_SECRET`, `SHAREPOINT_HMAC_SECRET`) or, on macOS, from
the keychain entries `sanctum-bridge-cf-access-client-{id,secret}` and
`sanctum-bridge-hmac-sharepoint`. A daily launchd timer
(`com.sanctum.bridge-manifest-sync`) keeps the file fresh on manoir.
