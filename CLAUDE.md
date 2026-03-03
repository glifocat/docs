# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Multi-project documentation portal for **glifocat** open source projects, built on **Mintlify**. Currently hosts the **ED1 HOAS** (ESPHome Home Assistant Starter) project documentation. Designed to scale — each new project gets its own folder and tab entry.

- **Domain:** `docs.glifo.cat`
- **Theme:** Tokyo Night color scheme (primary `#7aa2f7`, dark bg `#1a1b26`)
- **Repo:** `glifocat/docs`
- **License:** Apache 2.0

## Development

There is no build step, test suite, or linter. Mintlify processes MDX files directly and deploys from git.

**Preview locally:**
```bash
npx mintlify@latest dev
```

**Deployment:** Push to `main` → Mintlify auto-deploys.

## Architecture

### Site Configuration

`docs.json` is the single source of truth for navigation, theming, and site metadata. All navigation changes (adding pages, groups, tabs) happen here.

### Navigation Pattern

- **Tabs** = projects (top-level navigation)
- **Groups** = sections within a project (sidebar)
- **Pages** = individual MDX files (referenced by path without `.mdx`)

### Adding a New Project

1. Create a folder at root (e.g., `homelab/`)
2. Add MDX pages inside it
3. Add a `tab` entry in `docs.json` under `navigation.tabs`

### Adding a New Page to an Existing Project

1. Create the `.mdx` file in the appropriate project subfolder
2. Add the page path to the correct group in `docs.json`

### Content Conventions

- Every MDX page requires frontmatter with `title` and `description`
- Internal links use absolute paths from root (e.g., `/ed1-hoas/hardware/overview`)
- Images live in `/images/` with project prefix (e.g., `ed1-front.png`)
- Code examples in docs are primarily YAML (ESPHome configs, HA automations)

### Files Excluded from Mintlify

`.mintignore` excludes `drafts/`, `*.draft.mdx`, and `docs/plans/` from the published site. Mintlify also auto-ignores `.git`, `.github`, `.claude`, `README.md`, `LICENSE.md`, etc.

### Design Document

`docs/plans/2026-02-28-glifocat-docs-portal-design.md` contains the architectural decisions, migration plan, and Tokyo Night color palette reference.
