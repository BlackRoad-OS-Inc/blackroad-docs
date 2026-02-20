# CLAUDE.md - blackroad-docs

Documentation hub for BlackRoad OS, Inc. Pure Markdown + Mermaid diagrams.

## Commands

- Lint markdown: `npx markdownlint-cli2 "**/*.md"`
- Check links: `npx markdown-link-check **/*.md`

## Conventions

- Use ATX headers (`#` not underlines)
- Mermaid diagrams for all architecture visuals
- ADR format for decisions (`architecture/decisions/`)
- Keep files under 500 lines — split if longer
- Cross-reference with relative links
- No trailing whitespace
- One blank line between sections

## Structure

- `architecture/` — System design docs and ADRs
- `brand/` — Design system and visual guidelines
- `governance/` — Decision log, processes, policies
- `guides/` — How-to guides for developers
- `api/` — API specifications and protocols
- `roadmap/` — Planning and backlog
- `runbooks/` — Operational procedures

## Adding Documentation

1. Choose the correct directory based on content type
2. Use existing files as templates for formatting
3. Add cross-references to related docs
4. Update README.md table of contents if adding a new file
