# Contributing to Ravenswood Archive Stacks

Thanks for helping preserve BotC scripts! This guide covers how to add scripts to the archive.

## Script File Format

Each script file in `scripts/` is a **standard BotC JSON array** - the same format used by botc.app and other BotC tools:

```json
[
  { "id": "_meta", "name": "My Script", "author": "Author Name" },
  { "id": "washerwoman" },
  { "id": "librarian" },
  "empath",
  { "id": "custom_char", "name": "Custom", "team": "townsfolk", "ability": "..." }
]
```

### Rules

- First element must have `"id": "_meta"` with at least a `"name"` field
- Remaining elements are character references (strings or objects with `"id"`)
- File must validate against `schema/script.schema.json`

## Naming Convention

**Filenames are the MD5 hash of the file contents.** This ensures identical scripts are never duplicated.

To compute the correct filename:

```bash
# The hash of the file content becomes the filename
md5sum my-script.json
# Output: abc123...  my-script.json
# Rename to: scripts/abc123... .json
```

Or in Node.js:

```js
const crypto = require('crypto');
const fs = require('fs');
const content = fs.readFileSync('my-script.json', 'utf8').trim();
const hash = crypto.createHash('md5').update(content).digest('hex');
// Filename should be: scripts/${hash}.json
```

## Method 1: Pull Request (Manual)

Best for one-off contributions or sites without an API.

1. Fork this repo
2. Add your script files to `scripts/` (named by content hash)
3. Create or update your source manifest at `sources/{your-site}.json`:

```json
{
  "name": "Your Site Name",
  "url": "https://your-site.com",
  "contact": "you@example.com",
  "sync_method": "pull_request",
  "scripts": {
    "abc123...": {
      "source_id": "your-internal-id",
      "source_url": "https://your-site.com/scripts/your-internal-id",
      "synced_at": "2026-01-15T00:00:00Z"
    }
  }
}
```

4. Open a PR - CI will validate your scripts automatically

### What CI checks

- Filename matches MD5 hash of file contents
- Script validates against the JSON schema
- `_meta` block exists with a `name`
- Only your own `sources/{site}.json` is modified

## Method 2: Automated Sync (GitHub Action)

Best for sites with an export API endpoint.

1. Implement an export API on your site that returns paginated scripts:

```
GET /api/export-scripts?cursor={id}&limit=100

Response:
{
  "scripts": [
    {
      "hash": "md5-of-raw-json",
      "raw_json": [...],
      "source_id": "your-internal-id",
      "source_url": "https://your-site.com/scripts/id"
    }
  ],
  "next_cursor": "next-id-or-null"
}
```

2. Open a PR adding your `sources/{your-site}.json` with `"sync_method": "github_action"` and your `api_endpoint`
3. Once merged, the daily sync action will automatically pull your scripts

## Method 3: Direct Push (Trusted Contributors)

For high-frequency contributors. Request a deploy key by opening an issue. You'll get write access scoped to your `sources/{site}.json` and `scripts/`.

## Source Manifest Schema

Each source manifest (`sources/{site}.json`) tracks provenance:

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Display name of your site |
| `url` | Yes | URL of your site |
| `contact` | Yes | Contact email |
| `sync_method` | Yes | `"github_action"`, `"pull_request"`, or `"manual"` |
| `api_endpoint` | If sync_method=github_action | Your export API URL |
| `scripts` | Yes | Map of content hash to source metadata |

## Duplicate Scripts

If a script already exists in `scripts/` (same content hash from another source), that's fine — just add a reference in your source manifest. The `index.json` will list all sources for each script.

## Questions?

Open an issue on this repo.
