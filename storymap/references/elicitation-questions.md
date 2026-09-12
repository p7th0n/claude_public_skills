# Elicitation questions for story mapping

Organized by Jeff Patton's method. Each section splits questions into two
groups:

- **Ask yourself / Claude** — questions Claude should put to the user
  directly before drafting a map, because the user already knows the
  answer or the constraint.
- **Take to real users** — questions that only actual stakeholders/end
  users can answer. Claude should surface these as a list for the user to
  ask in a real conversation, not guess at them.

Pick 3-5 from the first group per session, plus a short list from the
second group relevant to the feature area at hand. Don't ask all of these
every time — match them to what's actually uncertain.

The example questions below use a community tool-lending library app as
flavor, since that's where this skill was first built and tested. Adapt
the specifics to whatever project you're actually mapping —
the categories and question *shapes* are the reusable part, not the
literal examples.

## Framing

The narrative arc: who's affected first, who's affected last, who's
affected most by this feature area.

**Ask yourself / Claude**
- Is this session about a single activity (e.g. reservations) or a whole
  release milestone (e.g. everything before the next real-user test round)?
- What triggered this — a piece of existing backlog/roadmap feedback, a
  new idea, or a gap noticed while building something else?

**Take to real users**
- Walk me through the last time you did this — what were you trying to get
  done, and what got in the way?
- What do you do today instead, if this doesn't exist yet (paper, texts,
  memory)?

## Personas

Roles beyond the obvious ones — and whether an existing role actually
splits further once you look closely (e.g. "admin" meaning different
things in a single-tenant vs. multi-tenant version of the same product).

**Ask yourself / Claude**
- Is an admin/manager role in one version of this product the same as in
  another, or does it need narrower/different permissions?
- Are we mapping for one persona this round, or comparing how two personas
  experience the same activity differently?

**Take to real users**
- What's your role, and how technical do you consider yourself with apps
  like this?
- What's the most frustrating part of how this is handled today?

## Backbone / activities

The spine of major activities in sequence — deliberately behavioral, never
CRUD-shaped (not "Manage Stands," but "Reserve a stand," "Show up and
hunt," "Log a harvest").

**Ask yourself / Claude**
- What's the sequence of activities a user moves through, start to finish,
  for this feature area? (e.g. discover a stand → reserve it → show up →
  log a harvest → review history)
- Which activity in that sequence is the one real users complain about
  most?

**Take to real users**
- What do you do right before this, and right after? (surfaces adjacent
  activities that don't belong on this map but matter for context)

## Walking skeleton

The thinnest end-to-end slice through the backbone that's still a real,
demoable path.

**Ask yourself / Claude**
- If we could ship only one path through this backbone and nothing else,
  which one proves the concept end-to-end?
- What's already true and stable elsewhere in the app that this skeleton
  can lean on, instead of rebuilding?

**Take to real users**
- (Not usually a real-user question — this is a build-sequencing call
  informed by the backbone above.)

## Story detail

Steps inside one activity. Use "as a [persona], I ___" only where it adds
clarity — not as a rigid template.

**Ask yourself / Claude**
- Within this one activity, what are the 3-6 concrete steps a user takes?
- Where does this story branch by persona (e.g. admin sees something
  different than a regular member)?

**Take to real users**
- When this goes wrong, what happens? What do you wish happened instead?

## Release slicing

What has to be true for MVP vs. what's genuinely later.

**Ask yourself / Claude**
- Of the stories on this map, which are load-bearing for the next
  real-user test round, and which are polish?
- Is there a dependency between stories that forces an ordering regardless
  of priority (e.g. can't log a harvest without a stand existing)?

**Take to real users**
- If you could only have one new thing from this list next month, which
  one?

## Assumption-surfacing

Prompts whose only job is to force "we don't actually know this yet" onto
the map instead of silently guessing.

**Ask yourself / Claude**
- What am I inferring here because it's plausible, versus because the
  user or a real stakeholder actually said it?
- Is this persona/activity shared across every version of this product,
  or specific to one — and are we sure, or assuming?
- If comparing against a sibling codebase that isn't actually checked out
  in this workspace, am I asserting something as fact that I only inferred
  from docs or conversation?
