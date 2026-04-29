# Sanctum Bridge — skill helper

This file is consumed by every Triptyq skill that needs to push artifacts to SharePoint (or any other Sanctum-bridged destination). It is the single source of truth for how skills talk to the bridge. Do not hardcode endpoint URLs or auth headers in individual skills — reference this helper instead.

> **Skill authors:** copy the relevant snippet below into your skill's "Upload to SharePoint" section, parameterizing only `DEST_FOLDER`, `DOC_TYPE`, and `IF_EXISTS`. Everything else stays as-is.

## Environment contract

The Claude sandbox must have the following env vars available before any bridge call. They are populated by Cowork from the user's secret store; skills don't ask the user for them.

| Variable                    | Purpose                                              |
|-----------------------------|------------------------------------------------------|
| `CF_ACCESS_ID`              | Cloudflare Access service token id                   |
| `CF_ACCESS_SECRET`          | Cloudflare Access service token secret               |
| `SHAREPOINT_HMAC_SECRET`    | HMAC signing key, scoped to the SharePoint module    |
| `SANCTUM_BRIDGE_BASE`       | Defaults to `https://bridge.nepveu.name`             |

If any of the first three is missing, the bridge call will 401. Skills should check and fail loudly with a pointer back to this helper, rather than silently producing a half-uploaded file.

## Destination allowlist

The SharePoint module rejects writes outside the structural folders defined in `triptyq-skills/sharepoint-structure.yaml`. Currently allowed roots:

- `Deals/<company>/...`            — pre-investment work
- `Portcos/<company>/...`          — portfolio company work
- `Fund Admin/...`                 — fund operations
- `LP/...`                         — LP communications and reporting
- `Internal/...`                   — Triptyq team docs

Anything else returns `403 destination_not_allowed`. If a skill needs a new root, add it to the allowlist file in `triptyq-skills` — don't try to bypass it.

## Upload action — bash invocation

This is the canonical snippet. Skills paste it under an "## Upload to SharePoint" heading and customize only the marked variables.

```bash
# --- skill-customized ---
DEST_FOLDER="Deals/${COMPANY}/Memos"   # or Portcos/${COMPANY}/Board, etc.
DOC_TYPE="investment-memo"             # see doc_type vocabulary below
IF_EXISTS="version"                    # version | overwrite | rename | fail
FILE_PATH="${OUTPUT_FILE}"
# --- end skill-customized ---

set -euo pipefail
: "${CF_ACCESS_ID:?missing — see _shared/sanctum-bridge.md}"
: "${CF_ACCESS_SECRET:?missing — see _shared/sanctum-bridge.md}"
: "${SHAREPOINT_HMAC_SECRET:?missing — see _shared/sanctum-bridge.md}"
BASE="${SANCTUM_BRIDGE_BASE:-https://bridge.nepveu.name}"

BODY=$(jq -nc \
  --arg folder "$DEST_FOLDER" \
  --arg name "$(basename "$FILE_PATH")" \
  --arg b64 "$(base64 -w0 "$FILE_PATH")" \
  --arg dt "$DOC_TYPE" \
  --arg ifx "$IF_EXISTS" \
  '{folder_path:$folder, filename:$name, content_b64:$b64,
    metadata:{doc_type:$dt}, create_folder_if_missing:true,
    if_exists:$ifx}')

TS=$(date -u +%Y-%m-%dT%H:%M:%SZ)
NONCE=$(uuidgen)
BODY_HASH=$(printf '%s' "$BODY" | sha256sum | cut -d' ' -f1)
TO_SIGN=$(printf '%s\n%s\nPOST\n/sharepoint/upload\n%s' "$TS" "$NONCE" "$BODY_HASH")
SIG=$(printf '%s' "$TO_SIGN" \
  | openssl dgst -sha256 -hmac "$SHAREPOINT_HMAC_SECRET" -hex \
  | awk '{print $2}')

RESP=$(curl -sS -w '\n%{http_code}' -X POST "$BASE/sharepoint/upload" \
  -H "CF-Access-Client-Id: $CF_ACCESS_ID" \
  -H "CF-Access-Client-Secret: $CF_ACCESS_SECRET" \
  -H "Authorization: SanctumHMAC v1" \
  -H "X-Sanctum-Module: sharepoint" \
  -H "X-Sanctum-Timestamp: $TS" \
  -H "X-Sanctum-Nonce: $NONCE" \
  -H "X-Sanctum-Signature: $SIG" \
  -H "Content-Type: application/json" \
  --data "$BODY")

CODE=$(printf '%s' "$RESP" | tail -n1)
JSON=$(printf '%s' "$RESP" | sed '$d')

if [[ "$CODE" != "200" ]]; then
  echo "Upload failed ($CODE): $JSON" >&2
  exit 1
fi

WEB_URL=$(printf '%s' "$JSON" | jq -r '.web_url')
echo "Uploaded → $WEB_URL"
```

## MCP-tool invocation (claude.ai custom connector)

When Claude is running in claude.ai chat (not the sandbox), call the MCP tool directly instead of curl:

```
sharepoint.upload(
  folder_path="Deals/Calder/Memos",
  filename="investment-memo-v3.docx",
  content_b64=...,
  metadata={"doc_type": "investment-memo"},
  if_exists="version"
)
```

Returns the same `{drive_item_id, web_url, version, etag}` shape.

## doc_type vocabulary

Pin a `doc_type` on every upload so SharePoint search and views can filter cleanly. Skills should pick from this list:

| doc_type              | Used by skill(s)                          |
|-----------------------|-------------------------------------------|
| `investment-memo`     | triptyq-investment-memo                   |
| `tearsheet`           | tearsheet-generator                       |
| `dd-summary`          | vc-due-diligence                          |
| `lp-update`           | lp-update-writer                          |
| `partners-meeting`    | partners-meeting-prep                     |
| `portco-status`       | portfolio-monitoring                      |
| `screening-note`      | deal-screening                            |
| `meeting-notes`       | (manual / Granola exports)                |
| `legal-doc`           | legal:* skills                            |

Adding a new value? Add it here and to `triptyq-skills/sharepoint-structure.yaml` in the same PR.

## `if_exists` semantics

| Value       | Behavior                                                  | When to use                          |
|-------------|-----------------------------------------------------------|--------------------------------------|
| `version`   | SharePoint native versioning, returns new version number  | Default — memos, tearsheets          |
| `overwrite` | Replaces the file, no version history                     | Auto-generated dashboards            |
| `rename`    | Appends `-1`, `-2`, ... to filename                       | When versions should be siblings     |
| `fail`      | Returns 409 if the file exists                            | Final/signed docs that should be immutable |

## Error reference

| Code | Meaning                              | Skill response                       |
|------|--------------------------------------|--------------------------------------|
| 401  | CF Access token missing/invalid      | Stop — env not set up. Tell user.    |
| 403  | `destination_not_allowed`            | Stop — destination off allowlist.    |
| 403  | `hmac_failed`                        | Stop — secret rotation likely needed.|
| 409  | `file_exists` (only with `if_exists=fail`) | Up to skill — usually surface to user. |
| 413  | Body too large (>250MB)              | Skill should split or compress.      |
| 429  | Graph rate limit                     | Retry with exponential backoff (helper handles this). |
| 5xx  | Bridge or Graph error                | Retry once, then surface to user.    |

## Manifest sync

This file is regenerated nightly by `triptyq-skills/scripts/sync-bridge-manifest.py` from `bridge.nepveu.name/_manifest`. Do not hand-edit the auto-generated sections (marked `<!-- AUTO -->` ... `<!-- /AUTO -->`). The narrative sections above (intro, allowlist, doc_type vocab, error reference) are hand-maintained.

<!-- AUTO -->
<!-- regenerated by scripts/sync-bridge-manifest.py — manifest sha256=ff70b8314416560a -->

## Available bridge modules

### `sharepoint` (v0.1.0)

#### `sharepoint.get_folder`

Look up a SharePoint folder by tenant-relative path.

- HTTP: `POST /sharepoint/folder`
- MCP tool: `sharepoint.get_folder`

**Request body**

```json
{
  "properties": {
    "path": {
      "title": "Path",
      "type": "string"
    }
  },
  "required": [
    "path"
  ],
  "title": "FolderRequest",
  "type": "object"
}
```

**Response**

```json
{
  "properties": {
    "drive_item_id": {
      "title": "Drive Item Id",
      "type": "string"
    },
    "web_url": {
      "title": "Web Url",
      "type": "string"
    },
    "child_count": {
      "title": "Child Count",
      "type": "integer"
    },
    "last_modified": {
      "title": "Last Modified",
      "type": "string"
    }
  },
  "required": [
    "drive_item_id",
    "web_url",
    "child_count",
    "last_modified"
  ],
  "title": "FolderInfo",
  "type": "object"
}
```

#### `sharepoint.upload`

Upload a file to SharePoint, creating folders if missing.

- HTTP: `POST /sharepoint/upload`
- MCP tool: `sharepoint.upload`

**Request body**

```json
{
  "properties": {
    "folder_path": {
      "description": "Tenant-relative SharePoint path. Must be under an allowlisted root.",
      "examples": [
        "Deals/Calder/Memos"
      ],
      "title": "Folder Path",
      "type": "string"
    },
    "filename": {
      "title": "Filename",
      "type": "string"
    },
    "content_b64": {
      "description": "Base64-encoded file body.",
      "title": "Content B64",
      "type": "string"
    },
    "metadata": {
      "additionalProperties": {
        "type": "string"
      },
      "title": "Metadata",
      "type": "object"
    },
    "create_folder_if_missing": {
      "default": true,
      "title": "Create Folder If Missing",
      "type": "boolean"
    },
    "if_exists": {
      "default": "version",
      "enum": [
        "version",
        "overwrite",
        "rename",
        "fail"
      ],
      "title": "If Exists",
      "type": "string"
    }
  },
  "required": [
    "folder_path",
    "filename",
    "content_b64"
  ],
  "title": "UploadRequest",
  "type": "object"
}
```

**Response**

```json
{
  "properties": {
    "drive_item_id": {
      "title": "Drive Item Id",
      "type": "string"
    },
    "web_url": {
      "title": "Web Url",
      "type": "string"
    },
    "version": {
      "anyOf": [
        {
          "type": "integer"
        },
        {
          "type": "null"
        }
      ],
      "default": null,
      "title": "Version"
    },
    "etag": {
      "title": "Etag",
      "type": "string"
    }
  },
  "required": [
    "drive_item_id",
    "web_url",
    "etag"
  ],
  "title": "UploadResponse",
  "type": "object"
}
```

<!-- /AUTO -->
