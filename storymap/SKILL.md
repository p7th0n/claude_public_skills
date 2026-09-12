---
name: storymap
description: Run a Jeff Patton-style user story mapping session to build shared understanding with stakeholders before writing a roadmap entry or spec proposal. Default mode covers one feature area per session; can also do an explicit whole-project "backbone-only" pass on request. Use when starting discovery on a new feature area, prepping for a conversation with real users/stakeholders, or when the user says "let's story map X," "run patton mode," or asks about personas/activities/user stories for a product.
allowed-tools:
  - Read
  - Write
  - AskUserQuestion
  - Bash(storymap render *)
---

# Storymap

This skill runs a discovery-stage story mapping session — the point is
shared understanding between whoever's building something and the people
who'll use it, not a backlog. Never treat its output as decided behavior,
and never produce code changes.

Rendering depends on the `storymap` CLI from
[MozaicWorks/storymap](https://github.com/mozaicworks/storymap) (MIT) — see
the repo root README for install instructions. `default.html.j2` (bundled
here) is this skill's own template, not MozaicWorks' stock one; drop
`--template` from the render commands below to get their original output
instead.

## Ground rules (Patton's method)

Infer, don't ask the user to write for you:

- **Personas** — who's affected, their role, tech comfort, goals and
  frustrations.
- **User activities** — the backbone: major things people do, in the
  order they naturally do them.
- **User stories** — concrete steps within each activity.
- **MVP release** and **future releases** — what's load-bearing now vs.
  genuinely later.

Organize entirely around user behavior. Never organize by technical
architecture — no "API," "Database," "Frontend," "Backend" sections. Where
real information is missing, write the assumption directly into the map
rather than inventing a confident answer. Output only StoryMap markdown —
no prose report wrapped around it, no code changes.

## Before drafting anything

1. Read `references/elicitation-questions.md` (bundled with this skill).
   Pick 3-5 questions relevant to the feature area at hand and ask the user
   directly. **If the `AskUserQuestion` tool isn't available in this
   session** (observed in practice when running non-interactively, e.g.
   via `claude -p`) — ask the same questions as plain text in your reply
   and stop there, waiting for the user's actual answer before drafting.
   Do not skip straight to inferring a map either way; asking first is the
   point of this skill over a bare prompt.
2. Also pull a short list (5-8) of plain-language questions from the same
   reference that the user could take to actual stakeholders/end users.
   Present these separately, clearly labeled as "for real users," since
   Claude cannot answer them on stakeholders' behalf.
3. If this project has more than one product/codebase sharing the same
   domain (check for things like a strategy doc, multiple deploy targets,
   or the user mentioning "our other app"), ask which one this session is
   about, or whether it's deliberately comparing both — don't assume
   silently. If comparing two codebases and only one is actually checked
   out in the current workspace, mark anything about the *other* one as an
   explicit assumption inferred from docs/conversation, not a verified
   fact — don't imply you read code you didn't.
4. Default scope is **one feature area per session**. If the user
   explicitly asks for a whole-project or multi-feature pass instead, see
   "Whole-project backbone mode" below rather than drafting full detail
   everywhere.
5. Before drafting Personas/Releases, read `storymap/_registry.md` if it
   exists in this project. Reuse existing canonical names verbatim rather
   than re-typing a close-but-different name for the same persona/release —
   unreconciled renames are exactly how two sessions end up describing the
   same thing under different names with nothing tying them together. If a
   persona or feature doesn't exist yet, add it to the registry in this same
   session rather than improvising silently.

## Whole-project backbone mode

Only when explicitly requested (e.g. "map the whole product," "just the
backbone across everything"). This is the exception, not the default:

- Keep it deliberately shallow — one representative task and one
  representative story per activity, not full detail. The goal is a spine
  to point at, not a competing map.
- If prior per-feature story maps already exist for parts of this project,
  reuse their findings and persona names rather than re-deriving them from
  scratch, and reference those files by name.
- State plainly, inside the output, that this is a high-level pass and
  that going deeper on any single activity should become its own
  per-feature session producing its own file — don't let one file grow
  into the whole project's full detail.
- Keep persona/activity naming consistent with any existing per-feature
  maps in this project so files stay comparable/mergeable later.

## Exact markdown syntax

The `storymap` CLI (`storymap render <file>`) parses a specific structure —
this is not free-form markdown, and getting the heading levels wrong
produces a file that "renders" with no warning but shows no content. Use
exactly this shape (confirmed against `storymap init`'s generated skeleton,
not guessed from a screenshot or example image):

```
# <Product/Feature Title>          <- h1, one line of description below

# Releases                        <- h1, literally "Releases"
## <Release Name>                 <- h2 per release, description text below

# Personas                        <- h1, literally "Personas"
## <Persona Name>                 <- h2 per persona
- **Role:** ...
- **Tech level:** ...
(description paragraph)

# Map                             <- h1, literally "Map"
## <Activity>                     <- h2, a column group
### <Task>                        <- h3, a column within the activity
#### <Story text> [status:: X] [persona:: Y] [release:: Z]   <- h4, a card
(description paragraph)
```

Field rules:
- `status` must be exactly one of `not-started | in-progress | done |
  blocked` — no other values (not `shipped`, not `unclear`).
- `release` must exactly match a release name (or its `id`, see below)
  defined under `# Releases`, or the story won't appear in any swimlane.
- Any release name containing a space must carry an `[id:: short-name]`
  field on its `## <Release Name>` heading (the CLI itself warns about this)
  — once assigned, story `[release::]` fields must reference the `id`, not
  the display text, so the heading can be renamed later without breaking
  references. Single-word names (`Shipped`, `Next`, `Later`) don't need one.
  If `storymap/_registry.md` exists in this project, check it for the full
  rule and a worked example.
- `persona` should match a name defined under `# Personas`.
- Activity (`##`) and Task (`###`) headings under `# Map` carry no body
  text of their own in the rendered output — put narrative content in the
  Story's description paragraph instead, not as loose text under an
  Activity/Task heading.
- `feature`, `issue`, and `decision` are optional bracket fields on story
  lines (`[feature:: slug]`, `[issue:: N]`, `[decision:: N]`) — the CLI
  silently ignores unrecognized keys, confirmed safe to add. Add `feature`
  only in whole-project-backbone-mode files or sessions spanning more than
  one feature; a single-feature session file doesn't need it (the file
  itself is already scoped to one feature — check `storymap/_registry.md`'s
  Features section, if present, for existing slugs). Add `issue`/`decision`
  only on stories that resolve or depend on a tracked issue-tracker or
  decision-log entry.

## Producing the map

- Write the map to `storymap/<slug>-storymap.md` in the current project
  (create the `storymap/` directory if it doesn't exist), where `<slug>`
  is a short kebab-case name for the feature area.
- When writing each story's description, follow `storymap_card_writing.md`
  (bundled with this skill) — sentence length, paragraph structure, bullet
  usage, and its editorial checklist — before saving the card.
- Before finishing, render with this skill's template:
  `storymap render storymap/<slug>-storymap.md --template ~/.claude/skills/storymap/default.html.j2`.
  This both self-checks that the file parses and produces HTML with the
  click-to-toggle story descriptions this skill always uses (hidden by
  default, revealed by clicking a story's name) — no separate
  post-processing step is needed. A warning like "document appears
  empty — no releases or activities found" means the heading structure
  above wasn't followed — fix it and re-render. Don't hand back a file
  that hasn't been validated this way; a silently-empty render is the
  single most common failure mode of this skill.
- If `storymap` isn't installed or the render step errors for an
  unrelated reason, say so plainly rather than silently skipping
  validation.
- As part of that same self-check, if `storymap/_registry.md` exists,
  compare this file's Persona and Release h2 headings against its canonical
  names. Flag (don't block on) anything that doesn't match verbatim — it's
  a signal of drift worth a one-line note to the user, not a hard failure.

## Closing every session

After the map, add a section titled `# Candidate roadmap entries` listing
only the specific gaps, decisions, or open questions this session
surfaced — not a restatement of the whole map. If this session follows up
on an existing decision-log entry or issue-tracker item, name it explicitly
in this section rather than leaving the connection implicit. The `storymap` CLI doesn't
recognize this section and will emit a harmless "unrecognised section"
warning for it when rendering — that's expected, it's for human review
only. This is a suggestion list for the user to review and manually
promote into wherever this project tracks decisions (a `ROADMAP.md`,
issue tracker, spec-proposal step, etc.) — this skill never edits those
artifacts itself, only suggests.
