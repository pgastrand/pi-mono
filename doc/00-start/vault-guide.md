---
aliases: ["Vault Guide", "vault-guide"]
type: guide
package: docs
topics:
  - navigation
  - metadata
cssclass: moc
tags:
  - pkg/docs
  - doc/guide
  - topic/navigation
  - topic/metadata
---

# Vault Guide

Related: [[home]] · [[graph-index]] · [[dashboards]] · [[tag-index]]

## Purpose

This folder is organized as an Obsidian vault for navigating pi monorepo documentation, generated API notes, package mirrors, workflow prompts, and planning notes.

## Recommended starting points

- `[[home]]` for the main entry point
- `[[graph-index]]` for topic and package clusters
- `[[dashboards]]` for Dataview-based exploration
- `[[../01-packages/coding-agent/home]]`, `[[../01-packages/mom/home]]`, `[[../01-packages/pods/home]]` for focused areas

## Note conventions

### Wiki links

Notes are connected with Obsidian-style links like `[[../01-packages/coding-agent/guides/sdk]]` and `[[../10-mirrors/ai/api/packages_ai_src_stream]]`.

### Frontmatter

Most notes include:

- `aliases` for nicer search names
- `type` such as `guide`, `api`, `index`, `prompt`, `plan`
- `package` such as `coding-agent`, `ai`, `agent`, `tui`, `mom`, `pods`
- `topics` for grouped navigation
- `tags` for search and Dataview

### Main note categories

- `type: index` for navigation hubs and dashboards
- `type: readme` for package entry docs
- `type: guide` for conceptual docs
- `type: api` for generated/source-oriented docs
- `type: prompt` for workflow prompt notes
- `type: plan` for planning docs
- `type: skill` for skill-related notes

## Using Dataview

If you use the Dataview plugin, start with `[[dashboards]]` and the package dashboards.

## Suggested Obsidian plugins

- Dataview
- Backlinks (built-in)
- Graph View (built-in)
- Tag Pane

## Best workflows

- Use Quick Switcher with aliases
- Pin `[[home]]`, `[[dashboards]]`, and your main package dashboard
- Use Graph View from `[[graph-index]]` to explore clusters
- Filter by `package` and `type` in Dataview-backed notes
