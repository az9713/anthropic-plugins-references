# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A static reference guide for all Anthropic Claude plugins and their individual skills. No build step, no dependencies, no server required — open `index.html` directly in a browser.

## Files

| File | Purpose |
|------|---------|
| `index.html` | **Main reference** — two-tab interface: "Plugin Overview" (card layout, quick reference) + "Skill Detail" (every individual skill, searchable, collapsible) |
| `README.md` | Project overview and source repo links |
| `.ignore/cc*.txt` | Session transcripts from the research agents that built this content |

## Source Repositories

The five Anthropic repos this guide covers:
- `https://github.com/anthropics/knowledge-work-plugins` — 18 office-profession plugins
- `https://github.com/anthropics/claude-plugins-official` — 44 developer-focused plugins
- `https://github.com/anthropics/financial-services` — financial services agents and vertical plugins
- `https://github.com/anthropics/claude-plugins-community` — 600+ community plugins
- `https://github.com/anthropics/claude-for-legal` — legal-practice plugins

## HTML File Conventions

`index.html` is fully self-contained (no external dependencies, no CDN). It uses:
- Two-tab interface: "Plugin Overview" tab + "Skill Detail" tab, switched via `switchTab()` JS
- Dark theme via CSS custom properties on `:root`
- Collapsible plugin sections (`.plugin-toggle` / `.plugin-body` with `.open` class) in the Skills tab
- Live search bar in the Skills tab that filters plugins/skills and auto-expands matches
- Color coding by source repo (blue/purple/green/amber/red)

When editing `index.html`, keep it self-contained — do not add external script or stylesheet links.

## Permissions

The `.claude/settings.local.json` allows fetching from `api.github.com` and `raw.githubusercontent.com`. Use these when re-researching or updating plugin content.
