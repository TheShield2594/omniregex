# omniregex

A single JSON ruleset (`omni.json`) that matches media release filenames against
regular expressions and maps each match to a display badge — a custom label,
an icon, a color, and border/opacity styling. It's a **data/config export**,
not an application: there is no source code or build in this repo, which is
why GitHub can't detect a primary language.

The ruleset is built for identifying and visually tagging release quality info
straight out of a filename or release name, covering:

- **Resolution** — 720p, 1080p, 2160p
- **HDR format** — SDR, HDR, HDR10, HDR10+, Dolby Vision, undefined HDR
- **Audio codec** — AAC, Dolby Digital, Dolby Digital+, DTS, DTS-HD, DTS-HD MA,
  DTS-X, TrueHD, Atmos
- **Channel layout** — 5.1, 6.1, 7.1
- **Source/release tier** — WEB, Blu-ray, and Remux, each split into three
  tiers (1–3) keyed off known encode/release group tags (e.g. `FraMeSToR`,
  `CtrlHD`, `BLURANiUM`, `NTb`, `EbP`, ...)

## Repo layout

```
omni.json          the ruleset (see schema below)
icons/icons/*.png   30 badge icons referenced from omni.json
```

The icons are referenced from `omni.json` via absolute raw GitHub URLs back
into this same repo (`raw.githubusercontent.com/TheShield2594/omniregex/...`),
so `omni.json` is self-contained and can be consumed from anywhere without
also fetching the icon files separately.

## `omni.json` schema

Top-level fields:

| Field | Type | Description |
|---|---|---|
| `version` | string | Ruleset format version (currently `1.1.0`) |
| `exportDate` | string | ISO 8601 timestamp of the export |
| `patterns` | array\<string\> | The list of regex patterns, in match order |
| `customNames` | object | pattern → display label (e.g. `"DTS-HD MA"`, `"🎭WEB SCENE"`) |
| `imageURLs` | object | pattern → badge icon URL |
| `colorHexValues` | object | pattern → badge color (hex) |
| `colorIndices` | object | pattern → color palette index, for patterns sharing a preset color |
| `borderRadiusIndices` | object | pattern → border-radius preset index |
| `borderThicknessIndices` | object | pattern → border-thickness preset index |
| `backgroundOpacities` | object | pattern → background opacity preset index |
| `tagEnabled` | array\<string\> | Subset of `patterns` currently enabled for tagging |
| `autoPlayEnabled` | array | Reserved/unused in this export (empty) |
| `defaultFilterEnabled` | array | Reserved/unused in this export (empty) |

Each pattern in `patterns` is a case-insensitive regex (most use lookaheads/
lookbehinds to disambiguate overlapping formats, e.g. distinguishing plain
`DTS` from `DTS-HD`, `DTS-HD MA`, and `DTS-X`). The same pattern string is
used as the shared key across `customNames`, `imageURLs`,
`colorHexValues`/`colorIndices`, and the border/opacity index maps, so a
consumer looks up all styling for a match by keying off the matched pattern.

## Usage

This repo is meant to be pulled in as a data file by a compatible consumer
(e.g. a userscript or app that scans release names and renders badges) rather
than run directly. There's no build step, dependency list, or entry point —
just update `omni.json` (and add icons under `icons/icons/` as needed) and
consumers that fetch this repo's raw content will pick up the change.

## License

MIT — see [LICENSE](LICENSE).
