# Foam Markdown Reference

Foam uses standard Markdown with Foam-specific links, embeds, properties, tags, block anchors, and query blocks. Do not copy Obsidian syntax blindly.

## Wikilinks

Basic forms:

```markdown
[[note-id]]
[[note-id|Display text]]
[[note-id#Heading]]
[[note-id#^block-id]]
[[#Heading in same note]]
```

Foam has two kinds of wikilinks:

1. Identifier links identify resources by name across the workspace: `[[note]]`, `[[folder/note]]`.
2. Path links start with `/` or `.`: `[[/folder/note]]`, `[[./note]]`, `[[../note]]`.

Use the shortest unambiguous identifier. If two notes share a filename, avoid ambiguous links like `[[todo]]`; use `[[house/todo]]` or a path link.

Foam supports directory links. `[[projects]]` opens `projects/index.md` or `projects/README.md`. If `projects.md` exists, the file takes priority over the folder.

Foam can generate Markdown link reference definitions at the bottom of notes, but Foam itself does not require them. Keep them off unless the workspace needs compatibility with non-Foam Markdown processors.

## Embeds

Embed a note, section, block, or image with `!` before a wikilink:

```markdown
![[note-id]]
![[note-id#Section]]
![[note-id#^block-id]]
![[image.png|300]]
![[image.png|300|center]]
![[image.png|400|Monthly sales chart]]
```

Foam supports embed modifiers:

```markdown
full![[note-id]]
content![[note-id]]
card![[note-id]]
inline![[note-id]]
full-card![[note-id]]
content-inline![[note-id#Section]]
```

`full` includes the title or heading. `content` excludes it. `card` renders a bordered container. `inline` renders the embed as part of the note.

## Block Anchors

Block anchors link to paragraphs, list items, headings, blockquotes, code blocks, and tables.

```markdown
This paragraph can be linked directly. ^key-finding

- A list item ^list-item
  - A nested item

## Methodology ^methodology
```

For an entire list, code block, table, or blockquote, put `^id` on its own line immediately after the block. One blank line is accepted for some block types.

Link or embed a block:

```markdown
[[research-notes#^key-finding]]
[[research-notes#^key-finding|See finding]]
![[research-notes#^key-finding]]
```

Use `foam rename block <note> <old-id> <new-id>` when changing block IDs that may be linked elsewhere.

## Tags

Inline tags:

```markdown
#project #project/active #machine-learning
```

Frontmatter tags:

```yaml
---
tags: [project, active]
---
```

Tags can be hierarchical with `/`. Use `foam tag list`, `foam tag search`, and `foam rename tag` for tag operations.

## Properties

Foam reads YAML frontmatter at the top of a file.

```yaml
---
title: "Project Alpha"
type: project
tags: [project, active]
alias: alpha, project-alpha
status: draft
---
```

Special Foam properties:

- `title`: display title in graph and references.
- `type`: resource type and graph styling dimension.
- `tags`: note tags.
- `alias`: alternative names for link autocomplete.

Foam's official docs use singular `alias`, not Obsidian's common `aliases` property.

For multi-word values in comma- or space-separated YAML scalars, quote the value or use dashes/underscores.

## Footnotes

Foam supports standard Markdown footnotes:

```markdown
This claim needs context.[^context]

[^context]: Supporting detail or citation.
```

## Foam Versus Obsidian

Important differences:

- Foam treats `[[folder/note]]` as an identifier link unless it starts with `/` or `.`.
- Foam supports directory links to `index.md` or `README.md`.
- Foam documents `alias` as the special alias property.
- Foam embeds support `full`, `content`, `card`, and `inline` modifiers.
- Foam query blocks are `foam-query` YAML or `foam-query-js`, not Dataview.
- Foam structural renames should use the CLI or VS Code commands so links are rewritten.
