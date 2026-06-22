# Templates And Queries

Templates and queries belong to the target Foam workspace, not this skill.

## Templates

Templates live in `.foam/templates/`.

Default note template priority:

1. `.foam/templates/new-note.js`
2. `.foam/templates/new-note.md`

Default daily note template priority:

1. `.foam/templates/daily-note.js`
2. `.foam/templates/daily-note.md`

Use Markdown templates by default. They are portable and safe.

```markdown
---
foam_template:
  name: New Note
  description: Standard note
  filepath: '$FOAM_CURRENT_DIR/$FOAM_SLUG.md'
---

# $FOAM_TITLE

$FOAM_SELECTED_TEXT
```

Daily note template example:

```markdown
---
type: daily-note
foam_template:
  description: Daily Note
  filepath: '/journals/$FOAM_DATE_YEAR-$FOAM_DATE_MONTH-$FOAM_DATE_DATE.md'
---

# $FOAM_DATE_YEAR-$FOAM_DATE_MONTH-$FOAM_DATE_DATE

## Notes
```

Useful Foam variables:

- `$FOAM_TITLE`
- `$FOAM_TITLE_SAFE`
- `$FOAM_SLUG`
- `$FOAM_SELECTED_TEXT`
- `$FOAM_CURRENT_DIR`
- `$FOAM_DATE_YEAR`, `$FOAM_DATE_MONTH`, `$FOAM_DATE_DATE`
- `${FOAM_DATE_FORMAT:YYYY-MM-DD}`

Use `FOAM_DATE_*` variables in daily note templates because they respect relative daily notes such as `/tomorrow`.

## JavaScript Template Safety

JavaScript templates are executable code.

Only use `foam note create --trust` when the user explicitly approves trusted workspace templates. Treat `new-note.js` and `daily-note.js` like scripts you would run manually.

Foam MCP refuses JavaScript templates. Markdown templates work across CLI, MCP, and VS Code without trust approval.

## Daily Notes

Create or inspect daily notes with the CLI:

```bash
foam daily
foam daily --create
foam daily --date 2026-06-22 --create
foam daily --create --path-only
```

Without a daily template, Foam creates `journals/YYYY-MM-DD.md`. A daily template can override both path and content.

## Foam Query Blocks

Use `foam-query` blocks for dynamic lists, tables, and counts in Markdown preview.

````markdown
```foam-query
filter: "#research"
sort: title ASC
limit: 10
```
````

Structured filters:

````markdown
```foam-query
filter:
  and:
    - tag: "#project"
    - not:
        tag: "#archive"
select: [title, tags, backlink-count]
sort: backlink-count DESC
format: table
```
````

Useful filters:

- `"#tag"`: notes with tag.
- `"[[note-id]]"`: notes linked to or from a note.
- `"/regex/"`: notes whose path matches regex.
- `"*"`: all notes.
- `tag`, `type`, `path`, `title`, `links_to`, `links_from`, `jexl` for structured YAML filters.

Use `$current` in `links_to` or `links_from` for queries relative to the note containing the block.

````markdown
```foam-query
filter:
  links_to: "$current"
```
````

## Saved Queries

Saved queries live in `.foam/queries/<id>.yaml` and use the same YAML body as `foam-query` blocks.

```yaml
name: Work in Progress
description: Notes currently being edited
filter:
  and:
    - tag: "#wip"
    - not:
        tag: "#archive"
sort: title ASC
limit: 50
```

Use the CLI when supported by the installed version:

```bash
foam query list
foam query show work-in-progress
foam query run work-in-progress --format json
```

## JavaScript Query Safety

Prefer `foam-query` with YAML and Jexl filters.

`foam-query-js` requires a trusted VS Code workspace and runs with editor permissions. Do not create or modify `foam-query-js` blocks unless the user explicitly accepts trusted JavaScript execution.
