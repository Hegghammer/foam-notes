# Foam Notes Skill

A lightweight agent skill for working with Foam workspaces.

The skill helps agents read, search, create, edit, rename, link, tag, and validate Foam notes with the official `foam` CLI while respecting Foam's Markdown dialect. It gives the agent concise guidance for common Foam tasks such as:

- Finding notes, tags, backlinks, placeholders, or orphaned notes.
- Creating regular notes and daily notes.
- Renaming or moving notes while preserving Foam links.
- Editing Foam Markdown with wikilinks, embeds, block anchors, tags, properties, templates, and queries.

## Contents

- `SKILL.md` - Main skill instructions.
- `references/foam-cli.md` - Foam CLI command patterns for agents.
- `references/foam-markdown.md` - Foam-specific Markdown syntax and Obsidian differences.
- `references/templates-and-queries.md` - Foam templates, daily notes, saved queries, and trust guidance.

## Setup

Install the Foam CLI:

```bash
npm install -g foam-cli
```

Then install this skill in your agent harness. The easiest way is simply to provide the agent with the URL to this repository and tell it to install the skill for you. 

## Relationship with MCP

You do not need to configure Foam MCP to use this skill. The skill is designed to work with the `foam` CLI on its own. Foam MCP is optional and can be useful in harnesses that support MCP-based workspace browsing.
