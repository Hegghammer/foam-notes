---
name: foam-notes
description: Work with Foam workspaces using the official Foam CLI and Foam-flavored Markdown. Use when reading, searching, creating, editing, linking, renaming, tagging, validating, or maintaining Foam note collections.
---

# Foam Notes

Use the official `foam` CLI as the backbone for Foam workspace operations, and write notes in Foam-flavored Markdown. Foam is not Obsidian: its wikilink resolution, embeds, properties, templates, and query blocks have their own behavior.

## First Steps

1. Confirm the workspace root. Prefer an explicit `--workspace <dir>` when operating outside the current directory. `FOAM_WORKSPACE` is also supported.
2. Use `foam --help` and `foam <command> --help` when command details matter. The installed CLI is the source of truth.
3. Prefer `--format json` when you need to parse command output or make follow-up decisions.
4. Use the CLI for structural operations because it understands Foam identifiers and rewrites links.

## Core Workflow

Inspect before changing:

```bash
foam list notes --workspace /path/to/notes
foam note show my-note --workspace /path/to/notes
foam links my-note --workspace /path/to/notes
```

Create through Foam:

```bash
foam note create --title "My Note" --workspace /path/to/notes
foam daily --create --workspace /path/to/notes
```

Rename, move, and delete through Foam:

```bash
foam rename note old-note new-note --workspace /path/to/notes
foam note move old-note --to archive/old-note.md --workspace /path/to/notes
foam note delete old-note --workspace /path/to/notes
```

Validate after changes:

```bash
foam lint --workspace /path/to/notes
foam list placeholders --workspace /path/to/notes
```

## Command Selection

Use `foam note show <id>` to inspect metadata, `foam note show <id> --content` to read content, and `foam outline <id>` to inspect long notes.

Use `foam links <id>` for incoming and outgoing links, `foam links <id> --incoming` for backlinks, and `foam links --path <path>` when identifier resolution is ambiguous.

Use `foam search <query>` for title, alias, tag, property, and type searches. Use `foam grep <pattern>` for full-text content search.

Use `foam tag list`, `foam tag search <tag>`, and `foam rename tag <old> <new>` for tag work.

Use `foam list orphans`, `foam list deadends`, `foam list placeholders`, and `foam graph` for graph hygiene.

Use `foam rename section` and `foam rename block` instead of manual heading or block-anchor renames when links may point to them.

See [Foam CLI Reference](references/foam-cli.md) for more command patterns.

## Foam Markdown Rules

Use standard Markdown plus Foam features:

```markdown
[[note-id]]
[[note-id#Heading]]
[[note-id#^block-id]]
[[note-id|Display text]]
![[note-id]]
content-card![[note-id#Section]]
#tag #nested/tag
```

Foam wikilinks can be identifier links or path links. `[[folder/note]]` is an identifier unless it starts with `/` or `.`. Use `[[/folder/note]]`, `[[./note]]`, or `[[../note]]` for path references.

Foam supports directory links: `[[projects]]` can resolve to `projects/index.md` or `projects/README.md`, unless a `projects.md` file takes priority.

Foam uses YAML frontmatter for properties. Official docs name the alias property `alias`:

```yaml
---
title: "Readable Graph Title"
type: project
tags: [project, active]
alias: alpha, project-alpha
---
```

See [Foam Markdown Reference](references/foam-markdown.md) for syntax details and Obsidian differences.

## Templates And Queries

Templates live in `.foam/templates/` inside the target workspace. Do not keep workspace templates in this skill.

Prefer Markdown templates. JavaScript templates are executable code. Only use `foam note create --trust` when the user explicitly confirms the workspace templates are trusted.

Daily notes are created with `foam daily --create`. Without a daily template, Foam uses `journals/YYYY-MM-DD.md`. A `.foam/templates/daily-note.md` or `.js` template can override both path and content.

Foam query blocks use `foam-query` YAML for dynamic preview results and `.foam/queries/*.yaml` for saved queries. Prefer `foam-query` and Jexl filters over `foam-query-js` unless the user explicitly accepts trusted JavaScript.

See [Templates And Queries](references/templates-and-queries.md) for safe examples.

## Safety Rules

Do not manually rename or move note files when Foam links may target them. Use `foam rename note` or `foam note move`.

Do not permanently delete notes unless the user explicitly asks. `foam note delete` moves notes to `.foam/trash/` by default; `--permanent` is destructive.

Do not pass `--trust` or create `foam-query-js` blocks unless the user explicitly approves trusted code execution.

When editing content directly, resolve the file path with `foam note show` or `foam note id` first, edit the Markdown file, then run `foam lint` and check placeholders if links changed.

When links are ambiguous, use `--path <path>` or the shortest unambiguous Foam identifier shown by `foam note id`.

## External References

For upstream details, consult the official Foam repository docs under `docs/user/tools/cli.md` and `docs/user/features/`.

Install or update the CLI with `npm install -g foam-cli`.
