# Foam CLI Reference

The `foam` command is the primary interface for workspace operations. Run `foam --help` and `foam <command> --help` for the installed version's exact options.

## Workspace Targeting

Foam chooses the workspace in this order:

1. `--workspace <dir>`
2. `FOAM_WORKSPACE`
3. Current directory

Use explicit `--workspace` in automation unless already running inside the workspace.

```bash
foam list notes --workspace /path/to/notes
FOAM_WORKSPACE=/path/to/notes foam list tags
```

Use `--format json` when you need structured output.

```bash
foam links my-note --format json --workspace /path/to/notes
```

## Read Operations

List notes and resources:

```bash
foam list notes
foam list notes --tag project
foam list notes --type daily-note
foam list tags --sort count
foam list templates
```

Inspect a note:

```bash
foam note show my-note
foam note show my-note --content
foam note show --path notes/my-note.md
foam note id --path notes/my-note.md
foam outline my-note
```

Inspect links:

```bash
foam links my-note
foam links my-note --incoming
foam links my-note --outgoing
foam links --path notes/my-note.md --format json
```

Search:

```bash
foam search "meeting"
foam search --tag project --tag active
foam search --property status=draft
foam search --property due
foam grep "action item" --context 2
```

Graph hygiene:

```bash
foam list orphans
foam list deadends
foam list placeholders
foam graph > graph.json
foam graph --include-placeholders
```

Saved queries, when supported by the installed CLI:

```bash
foam query list
foam query show work-in-progress
foam query run work-in-progress --format json
```

## Write Operations

Create notes:

```bash
foam note create --title "Meeting Notes"
foam note create --title "Meeting Notes" --dir meetings
foam daily --create
foam daily --date 2026-06-22 --create
foam daily --create --path-only
```

Use `foam note create --trust` only when the user explicitly approves trusted JavaScript templates.

Move and rename notes:

```bash
foam rename note old-name new-name
foam rename note my-note my-note --target-path archive/
foam note move my-note --to archive/my-note.md
```

Rename linkable targets:

```bash
foam rename tag project/active project/in-progress
foam rename section my-note "Background" "Context"
foam rename block my-note key-insight better-insight
```

Delete notes:

```bash
foam note delete old-draft
foam note delete old-draft --force
```

By default, Foam moves deleted notes to `.foam/trash/`. Only use `--permanent` when explicitly requested.

## Validation

Run lint after meaningful edits:

```bash
foam lint
foam lint --format json
foam lint --fix
```

`foam lint` exits with `2` when issues are found and no fix was applied. Treat that as a diagnostic result, not necessarily a command failure.

Check placeholders after link edits:

```bash
foam list placeholders
```

## MCP

Foam can expose a workspace over MCP:

```bash
foam mcp --workspace /path/to/notes
foam mcp --workspace /path/to/notes --allow-writes
```

The MCP server is read-only by default. `--allow-writes` lets agents create, edit, delete, move notes, and manage tags.

Do not assume MCP is configured for the current client. If it is not, use the CLI directly.
