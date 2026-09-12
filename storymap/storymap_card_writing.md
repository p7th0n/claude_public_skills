---
name: storymap-card-writing
description: Apply when writing or editing the description text inside a Story Map card. Renderer markdown-parses the description, so blank lines = paragraph breaks and lines starting with "- " become list items.
---

## Style rules

- Sentences: 10-20 words average, 30-word ceiling
- One idea per paragraph, 2-4 sentences max
- Blank line between paragraphs (renderer treats it as a break — no blank line means it reads as one run-on block)
- Use `- ` bullets for anything with 3+ discrete items (files touched, decisions, edge cases)
- Keep file paths and identifiers in backtick-free plain text as already styled — don't wrap in prose

## Editorial pass (before saving the card)

1. Does paragraph 1 state _what_ shipped, in one sentence?
2. Does paragraph 2 (if present) cover _how_ — implementation, files, key decision?
3. Any sentence over 30 words? Split it.
4. Any point said twice across paragraphs? Cut the repeat.
5. Would a bullet list read faster than this sentence? Convert it.
