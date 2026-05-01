---
name: triptyq-sharepoint-docx-editor
description: |
  Edit existing .docx files stored in SharePoint/OneDrive by directly manipulating OOXML in Claude's sandbox. Use this skill when the file already exists in SharePoint and needs surgical edits — adding sections, fixing text, inserting hyperlinks, updating data — without recreating the document from scratch.

  Triggers: "edit the memo in SharePoint", "add a section to the existing doc", "update the Word file on SharePoint", "fix the text in the docx", "add hyperlinks to the sources", "insert a competition section into the memo", "update the date in the memo".

  DO NOT use this skill to create a new .docx from scratch (use Anthropic's docx skill + docx-js instead), or when the file is local on Claude's machine (use Anthropic's docx skill + unpack.py instead).
---

See full implementation and documentation at:
https://github.com/Ogilthorp3/docx-xml-editor

The SKILL.md with complete workflow is in that repo at `/SKILL.md`.

## Quick decision guide

| Scenario | Use |
|----------|-----|
| Creating a new .docx from scratch | Anthropic docx skill (docx-js) |
| File is on Claude's local machine | Anthropic docx skill (unpack.py) |
| **File is in SharePoint/OneDrive** | **This skill** |
| **Editing existing memo or report** | **This skill** |
| **Adding hyperlinks to existing doc** | **This skill** |

## Triptyq SharePoint Infrastructure

- **Drive ID**: `b!wPs8sWIq70Kt7RsL80wFKBqB2HWWzZdAnhoakfwfYIWRgc_-ZgmSTLpthkkQlSJu`
- **Memo folder**: `/03_Occasions Invest/02_Deal Flow/[###]_[Company]/From Triptyq/`

### Download (read file from SharePoint)
Use Microsoft 365 MCP tools:
1. `sharepoint_search` to find the file by name/folder
2. `read_resource` with the returned `file:///` URI to get file content

### Edit (manipulate OOXML)
Work in Claude's local sandbox (`/home/claude/`):
1. Unzip the .docx (it's a zip of XML files)
2. Edit the XML directly (e.g., `word/document.xml` for body content)
3. Re-zip into a valid .docx

### Upload (write file back to SharePoint)
Use the GitHub Actions File Bridge:
1. Base64-encode the edited .docx
2. Create a `manifest.json` with filename, sharepoint_folder, conflict_behavior: "replace"
3. Commit both to `staging/<uuid>/` in the triptyq-skills repo
4. GitHub Action auto-uploads to SharePoint and cleans up
