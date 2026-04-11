# Ravenswood Archive Stacks

A community-owned, content-addressed archive of [Blood on the Clocktower](https://bloodontheclocktower.com/) scripts. Any BotC script site can contribute scripts here, ensuring no scripts are lost if any single service goes down.

## How It Works

- Each script is stored as a standard BotC JSON file in `scripts/`
- Filenames are the MD5 hash of the file contents (content-addressed = automatic de-duplication)
- Scripts are pure BotC JSON - usable directly in any BotC tool (botc.app, etc.)
- `index.json` provides a searchable index of all scripts with name, author, and source info

## Using the Archive

### Download everything

```bash
git clone https://github.com/Dan-314/ravenswood-archive-stacks.git
```

### Browse the index

[`index.json`](index.json) contains metadata for every script:

```json
[
  {
    "hash": "abc123...",
    "name": "Trouble Brewing",
    "author": "Steven Medway",
    "character_count": 25,
    "sources": ["ravenswoodarchive"]
  }
]
```

### Use a single script

Each file in `scripts/` is a standard BotC script JSON array. Download any `scripts/{hash}.json` and import it directly into your favourite BotC tool.

## Contributing Scripts

See [CONTRIBUTING.md](CONTRIBUTING.md) for full details. Three ways to contribute:

1. **Automated sync** — Expose an export API, register as a source, and scripts sync daily via GitHub Action
2. **Pull request** — Add script files to `scripts/` and update your `sources/{site}.json` manifest
3. **Direct push** — For trusted, high-frequency contributors with deploy key access

## Structure

```
scripts/           Raw BotC script JSON files, named by content hash
sources/           Source manifests — which scripts came from which site
schema/            JSON Schema for BotC script validation
index.json         Auto-generated searchable index
```

## License

[CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) — public domain. See [LICENSE](LICENSE).

Blood on the Clocktower is created and owned by [The Pandemonium Institute](https://www.thepandemoniumInstitute.com/).
