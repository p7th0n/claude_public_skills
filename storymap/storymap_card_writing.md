---
name: storymap-card-writing
description: Apply when writing or editing the description text inside a Story Map card. Renderer markdown-parses the description, so blank lines = paragraph breaks and lines starting with "- " become list items.
---

Apply `prose-style`'s base rules, plus these renderer-specific and card-specific additions.

## Renderer specifics

- Blank line between paragraphs — the renderer markdown-parses the description, so no blank line means it reads as one run-on block.
- Use `- ` bullets for anything with 3+ discrete items (files touched, decisions, edge cases) — renders as a real list.
- Keep file paths and identifiers in backtick-free plain text as already styled — don't wrap in prose.

## Editorial pass (before saving the card, on top of prose-style's)

1. Does paragraph 1 state _what_ shipped, in one sentence?
2. Does paragraph 2 (if present) cover _how_ — implementation, files, key decision?
