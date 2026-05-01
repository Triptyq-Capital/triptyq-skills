# Changelog

## v0.1.1 — 2026-05-01

Plugin packaging fix. Skill folders moved from repo root into `skills/` subdirectory so Cowork's plugin auto-discovery finds them. Removed the unsupported `skills` array from `plugin.json` (only `commands`, `agents`, `hooks`, `mcpServers` are valid manifest path overrides). No skill content changes — same 7 skills as v0.1.0.

## v0.1.0 — 2026-04-29

First packaged release. Migrates all Triptyq skills off Composio File Bridge onto the Sanctum bridge for SharePoint uploads.

**Skills (7)**
- `tearsheet-generator` — one-page company profiles
- `investment-memo` — IC memos with Sid Lee-inspired design
- `vc-due-diligence` — full DD with internal + external research
- `lp-update-writer` — quarterly LP letters and fund updates
- `partners-meeting-prep` — weekly Dealflow & Portcos agenda + compte-rendu
- `portfolio-monitoring` — portco status dashboards and board prep
- `deal-screening` — 15-minute pipeline filter

**Shared helpers (`_shared/`)**
- `sanctum-bridge.md` — bridge upload pattern + canonical `doc_type → folder` table
- `research-passes.md` — Outlook + SharePoint + Drive + Granola + External research pattern
- `partners.yaml` — partner → Google account map (Bert + Charles populated, Guillaume TBD)

**Repository configuration**
- `sharepoint-structure.yaml` — bridge allowlist source-of-truth, references Charles's canonical SP layout
- `.claude-plugin/plugin.json` — plugin manifest for Claude Cowork installation

**Known gaps**
- `sharepoint-filing` skill not yet migrated — still references Composio
- `triptyq-sharepoint-docx-editor` (in repo as `docx-xml-editor`) handles surgical OOXML edits separately from the bridge upload path
- Bridge `metadata.patch_failed` warning on uploads — SharePoint columns for `doc_type` / `company` are not yet defined
