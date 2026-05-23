# Anthropic Plugins Reference

A plain-English reference guide for every plugin and skill across Anthropic's official Claude plugin repositories.

## What's in This Repo

| File | Description |
|------|-------------|
| `plugin-directory.html` | High-level overview of all plugins across the 4 repositories |
| `plugin-skills-detailed.html` | **Main reference** — every individual skill in every plugin, searchable |
| `README.txt` | Original list of source repositories |

Open the HTML files in any browser — no server needed, no dependencies.

---

## The Journey

The 4 source repositories contain hundreds of Claude plugins spanning sales, finance, legal, engineering, and more. Most documentation is written for technical professionals using domain-specific language. The goal of this project was to translate all of it into plain English, organized by individual *skill* (the smallest callable unit in a plugin), so anyone — not just professionals — can understand what these tools do and how to use them personally.

### Phase 1 — High-level overview (`plugin-directory.html`)

A parallel team of 4 research agents fetched the GitHub repos simultaneously using the GitHub API and raw file URLs. Each agent covered one repo:

- `knowledge-work-plugins` — 18 plugins for office professions (sales, marketing, HR, legal, engineering, etc.)
- `claude-plugins-official` — 44 developer-focused plugins (code review, git workflow, LSP integrations, etc.)
- `financial-services` — 19 plugins for Wall Street (investment banking, equity research, PE, fund admin, etc.)
- `claude-plugins-community` — 600+ community-contributed plugins

The first output was a card-based HTML directory with a quick-reference table mapping personal use cases to plugins.

### Phase 2 — Skill-level deep dive (`plugin-skills-detailed.html`)

The high-level overview missed individual skills. A second research pass drilled into every plugin's skill files using the GitHub API directory listings and raw file content. A second agent team documented each skill with:

- **What it does** — plain English, no jargon
- **When to use it** — personal and professional scenarios
- **Output** — what you actually get

The final file covers 200+ skills across all 4 repos, with:
- Collapsible plugin sections (click to expand)
- Live search bar that auto-expands matching plugins
- Color-coded by repo (blue / purple / green / amber)
- Dark theme, no external dependencies

---

## Source Repositories

- https://github.com/anthropics/knowledge-work-plugins
- https://github.com/anthropics/claude-plugins-official
- https://github.com/anthropics/financial-services
- https://github.com/anthropics/claude-plugins-community

---

## Quick Reference: Best Plugins for Personal Use

| Goal | Plugin |
|------|--------|
| Manage tasks & memory | Productivity Plugin, auto-memory |
| Read & sign PDFs privately | PDF Viewer Plugin |
| Run a small business | Small Business Plugin |
| Write content / blog / social | Marketing Plugin |
| Review contracts | Legal Plugin |
| Learn to code | learning-output-style, math-olympiad |
| Better git commits | commit-commands |
| Research stocks | market-researcher, earnings-reviewer |
| Build a financial model | model-builder |
| Browse 600+ more tools | claude-plugins-community |
