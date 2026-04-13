# Story Extraction System v1

## Purpose

Build a system that helps Nate tell his story visually without turning storytelling into another exhausting manual chore.

This is not a "content machine."
This is a way to:
- catch real moments
- preserve context
- surface meaning
- assemble rough narrative material
- make visual storytelling easier to return to

## Core principle

**Story first, tooling second.**

The system should not try to auto-generate polished videos on day one.
Its first job is to prevent good moments from disappearing into clip piles.

## What the system needs to do

1. ingest media from multiple devices
2. normalize and organize it
3. transcribe spoken/audio content
4. extract possible story beats
5. group material into candidate stories
6. produce rough narrative packets for review
7. eventually support rough-cut generation

---

# 1. Inputs

## Capture sources

Primary expected sources:
- iPhone video clips
- iPhone photos
- iPad recordings or screenshots
- Plaud recordings
- voice memos
- screen recordings
- short typed notes
- Mac screenshots / screen captures

## Typical raw material

- short clips from the day
- in-the-car reflections
- build updates
- workshop / terminal / code moments
- fatherhood logistics and small wins
- work-life contrast moments
- civic work notes
- environmental texture / B-roll

---

# 2. System roles by machine

## M4 Air
Role:
- main operator machine
- planning, review, metadata cleanup
- OpenClaw orchestration
- story review and tagging

## i7 Air (Jeep machine)
Role:
- field machine
- light capture support
- quick notes, light review, emergency edits
- not the main processing box

## M1 MBP
Role:
- story shop
- heavy ingest/transcription processing
- ffmpeg / media transforms
- rough cut generation
- export worker

## NAS
Role:
- archive
- central media store
- processed output sink
- cross-machine handoff location

## iPhone / iPad
Role:
- capture front end
- browsing / reviewing / lightweight notes

---

# 3. Folder model

## Top-level structure

```text
story-system/
  inbox/
  processing/
  candidates/
  exports/
  archive/
  logs/
```

## Folder purpose

### `inbox/`
Raw, newly arrived material.

### `processing/`
Temporary workspace while transcription, metadata extraction, thumbnails, and grouping happen.

### `candidates/`
Actual story packets worth review.

### `exports/`
Rendered or assembled output.

### `archive/`
Long-term storage for processed/finished material.

### `logs/`
Processing notes, manifests, run logs.

## Candidate story folder shape

```text
candidates/
  2026-04-12_building-before-shift/
    media/
    transcripts/
    thumbnails/
    notes.md
    beats.md
    selects.md
    sequence.md
    metadata.json
```

---

# 4. Pipeline stages

## Stage 1 — Ingest

### Goal
Get raw material into one place reliably.

### Actions
- copy new files into `inbox/`
- preserve original files
- assign stable filenames
- record source device
- record created/modified timestamps

### Good v1 behavior
- date-prefixed filenames
- source tags in metadata
- no destructive moves yet

Example filename:
`2026-04-12_iphone_clip_001.mov`

---

## Stage 2 — Normalize

### Goal
Make the raw material machine-readable and human-browseable.

### Actions
- extract metadata
- create thumbnails/contact sheets
- detect media type
- standardize extensions if needed
- create transcript jobs for audio/video

### Outputs
- metadata json
- thumbnails
- transcript placeholders
- media manifest entry

---

## Stage 3 — Transcription

### Goal
Turn spoken material into searchable story material.

### Tools
Likely v1 options:
- local Whisper / Whisper CLI
- Plaud exports when available

### Outputs
- transcript text
- timestamped transcript if possible
- confidence notes if useful

### Why this matters
A huge amount of story signal will be in spoken reflections, not just visuals.

---

## Stage 4 — Story extraction

### Goal
Identify what might actually matter.

### Questions the system should answer per item
- what is happening here?
- what is this about?
- what project/life thread does it belong to?
- is there tension, change, conflict, humor, or a clear beat?
- is this a quote, scene, transition, texture shot, or likely discard?

### Expected output fields
For each item:
- summary
- themes
- tags
- possible hooks
- useful quotes
- emotional/practical beat
- storyworthiness score

### Example tags
- `build-life`
- `kitchen`
- `fatherhood`
- `jeep`
- `civic-work`
- `fatigue`
- `small-win`
- `transition`
- `system-building`

---

## Stage 5 — Group into story candidates

### Goal
Bundle related items into potential stories.

### Grouping logic
Candidates can be grouped by:
- date/time proximity
- repeated topic/theme
- same event
- shared transcript language
- same project or place

### Candidate story examples
- building before shift
- trying to work from the Jeep
- the friction between kitchen work and founder work
- the daily school/time/fatherhood logistics reality
- fixing systems across multiple machines

### Output
A candidate folder with:
- selected media
- transcripts
- notes
- candidate thesis/angle

---

## Stage 6 — Story packet generation

### Goal
Create something reviewable without editing from scratch.

### Packet contents
- one-paragraph summary
- why it matters
- best quotes
- best clips
- suggested opening hook
- possible sequence order
- short-form angle
- long-form angle

### What this gives Nate
Not a finished video.
A shaped pile instead of a chaotic pile.

---

## Stage 7 — Rough cut generation (later)

### Goal
Create draft assemblies once story extraction is working.

### Possible outputs later
- selected clip stringout
- transcript-led assembly
- narrated rough structure
- short-form cut suggestion

### Important
This is not v1.
Do not start here.

---

# 5. Story qualification rules

## A clip or note becomes story material if it has one or more of:
- tension
- change
- specific stake
- revealing line
- before/after
- contrast
- human truth
- useful texture supporting a stronger moment

## A clip is likely background/noise if it is:
- generic with no beat
- visually redundant
- contextless with no usable transcript signal
- something only meaningful because you remember it, not because it contains signal

---

# 6. Recurring story lanes to expect

These are probably the first buckets:
- building a way out of the kitchen
- the machine life, Macs, Jeep, terminals, mobility
- fatherhood and logistics
- fatigue vs ambition
- civic work vs real-world daily pressure
- tiny wins and system repairs
- trying to build under constraint

These are not branding categories.
They are actual recurring narrative threads.

---

# 7. Suggested v1 outputs

## For each good candidate story
Create:
- `notes.md`
- `beats.md`
- `selects.md`
- `sequence.md`
- transcript files
- thumbnails/contact sheets

## Example contents

### `notes.md`
What happened, why it matters, rough theme.

### `beats.md`
List of moments / turns.

### `selects.md`
Most useful clips and quotes.

### `sequence.md`
Possible order of scenes.

---

# 8. Tooling ideas by stage

## v1 likely tools
- file watchers / folder scripts
- ffmpeg
- Whisper / openai-whisper skill
- OpenClaw orchestration
- markdown story packets
- NAS as shared storage/archive

## Later tools
- scene detection
- clip scoring
- rough assembly generation
- subtitle burn-in
- social export variants

---

# 9. What success looks like

The system is working if:
- Nate captures more moments because there is a trustworthy path forward
- raw material is searchable and grouped
- story candidates appear without starting from zero
- review feels like choosing and shaping, not rescuing chaos
- storytelling becomes more likely, not less

## Failure mode to avoid
Building an elaborate "AI editor" that never helps tell a real story.

---

# 10. Best v1 build order

1. inbox folder structure
2. media ingest conventions
3. transcription pipeline
4. metadata + summary extraction
5. story candidate packet generation
6. manual review workflow
7. rough cut automation later

---

# 11. Recommendation

Build the first version around this promise:

**Drop media in. Get back a story packet.**

That is enough to change behavior.
That is enough to keep stories alive.
That is enough to justify the next layer later.
