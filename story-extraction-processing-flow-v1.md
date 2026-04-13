# Story Extraction Processing Flow v1

## Goal

Define the simplest version of the system that is actually worth using.

The job of v1 is not to create polished videos.
The job of v1 is to reliably turn raw media into **story packets**.

## Core promise

**Drop media in. Get back a story packet.**

That is the whole thing.
If v1 does that well, it earns the right to become more sophisticated later.

---

# v1 flow at a glance

## Step 1 — Capture
Raw media gets captured during normal life/work.

Examples:
- iPhone clip
- Plaud recording
- voice memo
- screenshot
- photo
- screen recording

No extra friction here.
Capture should stay simple.

## Step 2 — Drop into inbox
Everything lands in one intake location.

Examples:
- local `story-system/inbox/`
- NAS inbox mirror later

Rule:
- no sorting by hand before ingest
- no overthinking before ingest
- just drop it in

## Step 3 — Intake pass
The system notices new items and creates a basic entry for each one.

For every new file:
- assign a stable filename if needed
- record source device if known
- record timestamps
- detect media type
- create manifest entry

Output:
- item exists in processing manifest

## Step 4 — Derivatives pass
Create the useful support materials.

For video:
- thumbnail
- contact sheet later if needed
- transcript if speech exists

For audio:
- transcript

For image:
- metadata extraction
- optional OCR later if relevant

Output:
- transcript file if applicable
- thumbnail/preview if applicable
- metadata json

## Step 5 — Story extraction pass
This is the key move.

For each item, generate:
- short summary
- likely themes
- possible story value
- useful quotes if transcript exists
- type label

Suggested type labels:
- `scene`
- `quote`
- `broll`
- `note`
- `turn`
- `discard-maybe`

Suggested theme labels:
- `build-life`
- `kitchen`
- `fatherhood`
- `jeep`
- `systems`
- `fatigue`
- `small-win`
- `transition`

Output:
- one small story-analysis record per media item

## Step 6 — Candidate grouping pass
Group related items together into a possible story.

Grouping signals:
- same day
- same theme
- shared language in transcript
- same project
- same event or location

Output:
- candidate story folder or packet

## Step 7 — Story packet generation
For each promising candidate, generate a lightweight packet.

Packet should include:
- what happened
- why it matters
- best clips
- best quotes
- possible hook
- suggested structure
- next action

This is the handoff point for human review.

## Step 8 — Human review
Nate reviews packets and decides:
- keep
- merge
- ignore
- turn into short-form piece
- turn into longer narrative piece

That is where judgment stays human.

---

# What the system produces in v1

## Minimum outputs per item
- metadata file
- transcript if needed
- summary
- tags/themes
- storyworthiness note

## Minimum outputs per candidate story
- story summary
- best moments
- best quotes
- suggested hook
- rough sequence idea

That’s enough.

---

# What v1 does NOT do

Not yet:
- auto-final-edit videos
- auto-post content
- build a full dashboard
- perfect scene detection
- fancy embeddings or retrieval systems
- overcomplicated scoring models

If we do too much too early, this turns into a science project and dies.

---

# Human loop

## Human responsibilities
Nate still decides:
- what matters
- what is true
- what becomes public
- what belongs together
- what tone is right

## System responsibilities
The system should handle:
- organizing
- transcribing
- surfacing
- grouping
- drafting packet structure

This keeps the system useful without making it fake-smart.

---

# Good v1 success criteria

v1 is good enough if:
- dropping media into inbox reliably works
- transcripts appear automatically
- each item gets a decent summary and tags
- story packets appear for promising groups
- review is easier than starting from a giant pile
- Nate is more likely to keep capturing moments

---

# Failure criteria

v1 has failed if:
- ingest is annoying
- folder structure is confusing
- outputs are too noisy to trust
- packets are generic and useless
- workflow feels heavier than doing nothing

---

# Recommended implementation order

1. inbox
2. manifest
3. transcription
4. item-level story extraction
5. candidate grouping
6. story packet generation
7. human review loop

That is the whole machine in the right order.

---

# Best mental model

Do not think of this as:
- video editing automation

Think of it as:
- **narrative composting**

Raw scraps go in.
Meaningful story material comes out.
That’s the system.
