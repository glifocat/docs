# glifocat docs portal — Design document

**Date:** 2026-02-28
**Status:** Approved
**Repo:** github.com/glifocat/docs (local: `~/personal-projects/mintlify/docs/`)

## Summary

Repurpose the existing Mintlify starter kit repo (`glifocat/docs`) into a multi-project documentation portal for glifocat open source projects. Start with ED1 HOAS as the first (and only) project, using a tabs-based navigation structure that scales to additional projects over time.

## Goals

- Single Mintlify site at `docs.glifo.cat` for all glifocat projects
- Tokyo Night-inspired color scheme and branding
- Automatic LLM readability via Mintlify's `llms.txt` / `llms-full.txt`
- Context7 indexability for AI-assisted development
- Easy to add new projects — drop a folder, add a tab entry

## Architecture

### Navigation pattern: Tabs per project

Each project gets its own tab in the top navigation and its own folder in the repo. A portal landing page (`index.mdx`) lives outside any tab and serves as the entry point.

Adding a new project requires:
1. Create a folder (e.g., `homelab/`)
2. Add MDX pages inside it
3. Add a new `tab` entry in `docs.json`

### Repository structure

```
glifocat-docs/
├── docs.json                      # Site config, tabs, Tokyo Night theme
├── index.mdx                      # Portal landing page
├── ed1-hoas/
│   ├── introduction.mdx           # Migrated from ed1-hoas/docs/
│   ├── getting-started.mdx
│   ├── hardware/
│   │   ├── overview.mdx
│   │   └── pinout.mdx
│   └── esphome/
│       ├── configuration.mdx
│       ├── home-assistant.mdx
│       └── smartir.mdx
├── images/
│   ├── ed1-front.png              # Migrated from ed1-hoas/docs/images/
│   ├── ed1-back.png
│   └── ed1-board.png
├── logo/
│   ├── light.svg                  # glifocat branding (to create/source)
│   └── dark.svg
├── favicon.svg
└── snippets/                      # Shared reusable components (future)
```

## Theme & branding

**Name:** glifo.cat
**Domain:** `docs.glifo.cat` (to configure in Mintlify dashboard)

### Tokyo Night color scheme

| Role                | Color             | Hex       |
|---------------------|-------------------|-----------|
| Primary             | Tokyo Night Blue  | `#7aa2f7` |
| Light mode primary  | Tokyo Night Cyan  | `#7dcfff` |
| Dark mode primary   | Tokyo Night Blue  | `#7aa2f7` |
| Dark background     | Tokyo Night BG    | `#1a1b26` |

Reference palette (for future use in custom CSS):
- Red: `#f7768e` — errors, destructive actions
- Orange: `#ff9e64` — warnings, constants
- Yellow: `#e0af68` — tips, highlights
- Green: `#9ece6a` — success, strings
- Teal: `#73daca` — links, keys
- Cyan: `#7dcfff` — properties, headings
- Purple: `#bb9af7` — keywords, accents
- Foreground: `#a9b1d6`
- Comments: `#565f89`

## docs.json configuration

```json
{
  "$schema": "https://mintlify.com/docs.json",
  "theme": "mint",
  "name": "glifo.cat",
  "description": "Documentation for glifocat open source projects",
  "colors": {
    "primary": "#7aa2f7",
    "light": "#7dcfff",
    "dark": "#7aa2f7",
    "background": { "dark": "#1a1b26" }
  },
  "favicon": "/favicon.svg",
  "logo": {
    "light": "/logo/light.svg",
    "dark": "/logo/dark.svg"
  },
  "navigation": {
    "tabs": [
      {
        "tab": "ED1 HOAS",
        "groups": [
          {
            "group": "Getting started",
            "pages": ["ed1-hoas/introduction", "ed1-hoas/getting-started"]
          },
          {
            "group": "Hardware",
            "pages": ["ed1-hoas/hardware/overview", "ed1-hoas/hardware/pinout"]
          },
          {
            "group": "ESPHome",
            "pages": [
              "ed1-hoas/esphome/configuration",
              "ed1-hoas/esphome/home-assistant",
              "ed1-hoas/esphome/smartir"
            ]
          }
        ]
      }
    ],
    "global": {
      "anchors": [
        {
          "anchor": "GitHub",
          "href": "https://github.com/glifocat",
          "icon": "github"
        }
      ]
    }
  },
  "navbar": {
    "links": [
      { "label": "GitHub", "href": "https://github.com/glifocat" }
    ]
  },
  "footer": {
    "socials": {
      "github": "https://github.com/glifocat"
    }
  }
}
```

## Migration plan

### Content to migrate from ed1-hoas/docs/

| Source file                        | Destination                            |
|------------------------------------|----------------------------------------|
| `introduction.mdx`                | `ed1-hoas/introduction.mdx`           |
| `getting-started.mdx`             | `ed1-hoas/getting-started.mdx`        |
| `hardware/overview.mdx`           | `ed1-hoas/hardware/overview.mdx`      |
| `hardware/pinout.mdx`             | `ed1-hoas/hardware/pinout.mdx`        |
| `esphome/configuration.mdx`       | `ed1-hoas/esphome/configuration.mdx`  |
| `esphome/home-assistant.mdx`      | `ed1-hoas/esphome/home-assistant.mdx` |
| `esphome/smartir.mdx`             | `ed1-hoas/esphome/smartir.mdx`        |
| `images/ed1-front.png`            | `images/ed1-front.png`                |
| `images/ed1-back.png`             | `images/ed1-back.png`                 |
| `images/ed1-board.png`            | `images/ed1-board.png`                |

### Link updates required

All internal links in migrated MDX files need the `ed1-hoas/` prefix:
- `/hardware/overview` → `/ed1-hoas/hardware/overview`
- `/getting-started` → `/ed1-hoas/getting-started`
- `/esphome/configuration` → `/ed1-hoas/esphome/configuration`
- (etc.)

Image paths remain unchanged (`/images/ed1-*.png`).

### Content to delete (starter kit)

- `quickstart.mdx`
- `development.mdx`
- `essentials/` (entire directory)
- `ai-tools/` (entire directory)
- `api-reference/` (entire directory)
- `AGENTS.md`
- `CONTRIBUTING.md`
- `README.md` (replace with portal-specific README)

### Content to create

- `index.mdx` — portal landing page with project cards
- `logo/light.svg` and `logo/dark.svg` — glifocat branding (placeholder or designed)
- `favicon.svg` — updated favicon
- `.mintignore` — updated to exclude plan files, etc.

## LLM & AI indexing

Mintlify automatically generates:
- `/llms.txt` — structured index for LLM crawlers
- `/llms-full.txt` — full docs content in single file

No additional setup needed — these are generated on every deploy.

## ed1-hoas docs/ folder

Decision deferred. Options to revisit after portal is working:
1. Remove MDX files, keep hardware schematics (`docs/ED1 2.3/`)
2. Keep everything as-is (dual copies)
3. Remove all docs content

## Out of scope

- Custom domain DNS setup (requires Mintlify dashboard)
- Logo design (placeholder SVGs for now)
- Other project docs (homelab, LicitAlert, etc.) — add later
- Custom CSS beyond what docs.json supports
