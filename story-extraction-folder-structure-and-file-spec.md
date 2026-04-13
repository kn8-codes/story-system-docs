# Story Extraction Folder Structure and File Spec

## Goal

Define a folder structure that is simple enough to use, durable enough to scale, and clear enough that future automation does not turn into a swamp.

This spec is for v1 and early v2.
It should support:
- easy ingest
- safe processing
- readable outputs
- human review
- future automation on the MBP and NAS

---

# 1. Top-level directory layout

```text
story-system/
  inbox/
  processing/
  candidates/
  exports/
  archive/
  logs/
  config/
```

## Directory roles

### `inbox/`
Raw incoming material.
This is the only place humans should casually drop files.

### `processing/`
Temporary working area while files are being analyzed and transformed.

### `candidates/`
Story packets and grouped media that are worth human review.

### `exports/`
Finished or semi-finished output.
Examples:
- rough cuts
- selects reels
- final renders
- subtitle files

### `archive/`
Long-term retained source and processed materials.
This is where older completed work should live.

### `logs/`
Machine-readable and human-readable processing logs.

### `config/`
Shared settings, tag lists, prompt files, naming rules, and workflow config.

---

# 2. Inbox structure

## Principle
Inbox should stay dead simple.
Do not over-nest it.

## Recommended layout

```text
inbox/
  2026-04-13/
  2026-04-14/
```

Each date folder contains raw drops from any device.

## Naming rule for dropped files
If source filenames are ugly, normalize during intake.

Recommended normalized pattern:

```text
YYYY-MM-DD_source_mediaType_###.ext
```

Examples:
- `2026-04-13_iphone_video_001.mov`
- `2026-04-13_plaud_audio_001.m4a`
- `2026-04-13_ipad_screenshot_001.png`
- `2026-04-13_mbp_screenrec_001.mp4`

## Why date folders first
Because the first useful grouping dimension is often time.
It keeps intake easy and prevents instant chaos.

---

# 3. Processing structure

## Purpose
Hold work products while the system is extracting meaning.

## Recommended layout

```text
processing/
  items/
    <item-id>/
      source/
      derived/
      transcripts/
      metadata/
      analysis/
```

## Example

```text
processing/
  items/
    item_2026-04-13_iphone_video_001/
      source/
        2026-04-13_iphone_video_001.mov
      derived/
        thumbnail.jpg
        waveform.png
      transcripts/
        transcript.md
        transcript.json
      metadata/
        metadata.json
      analysis/
        summary.md
        story-analysis.json
```

## Why item-based folders
Every source asset becomes one atomic unit that can be safely processed, tracked, and debugged.

---

# 4. Candidate story structure

## Purpose
Store grouped material that may become an actual story.

## Recommended layout

```text
candidates/
  YYYY-MM-DD_slug/
    media/
    transcripts/
    references/
    packet/
    rough-cut/
    metadata/
```

## Example

```text
candidates/
  2026-04-13_building-before-shift/
    media/
      2026-04-13_iphone_video_001.mov
      2026-04-13_iphone_video_003.mov
    transcripts/
      combined-transcript.md
    references/
      stills-contact-sheet.jpg
    packet/
      notes.md
      beats.md
      selects.md
      sequence.md
      hook-ideas.md
    rough-cut/
      draft-edl.txt
      rough-notes.md
    metadata/
      story.json
```

## Slug rules
Use human-readable slugs.

Examples:
- `building-before-shift`
- `coding-in-the-jeep`
- `fatherhood-school-run`
- `kitchen-to-founder-gap`

Do not use opaque IDs for candidate story folders unless needed internally.

---

# 5. Exports structure

## Purpose
Hold outputs that can be reviewed or published.

## Recommended layout

```text
exports/
  YYYY-MM-DD_slug/
    short/
    long/
    captions/
    thumbnails/
```

## Example

```text
exports/
  2026-04-13_building-before-shift/
    short/
      vertical-cut-v1.mp4
    long/
      doc-fragment-v1.mp4
    captions/
      short-v1.srt
    thumbnails/
      short-cover-001.jpg
```

---

# 6. Archive structure

## Purpose
Store settled materials without clogging active work areas.

## Recommended layout

```text
archive/
  raw/
    2026/
      2026-04/
  processed/
    2026/
      2026-04/
  stories/
    2026/
      2026-04/
```

## Notes
Archive should be boring and reliable.
No cleverness.
This is where NAS integration matters later.

---

# 7. Logs structure

## Recommended layout

```text
logs/
  intake/
  transcription/
  extraction/
  grouping/
  exports/
```

## Example files
- `logs/intake/2026-04-13.log`
- `logs/transcription/2026-04-13.log`
- `logs/extraction/2026-04-13.log`

Use append-only logs where possible.

---

# 8. Config structure

## Recommended layout

```text
config/
  tags.json
  themes.json
  naming-rules.md
  prompts/
    item-summary.md
    story-packet.md
  workflow.json
```

## Purpose
Keep prompts, tags, and workflow settings out of code when possible.
This makes iteration easier.

---

# 9. File spec by type

## 9.1 `metadata.json`
Purpose:
Basic extracted facts about one media item.

Suggested shape:

```json
{
  "itemId": "item_2026-04-13_iphone_video_001",
  "sourceFile": "2026-04-13_iphone_video_001.mov",
  "sourceDevice": "iphone",
  "mediaType": "video",
  "capturedAt": "2026-04-13T05:21:00-04:00",
  "ingestedAt": "2026-04-13T05:30:00-04:00",
  "durationSeconds": 42.7,
  "sizeBytes": 18273642,
  "mimeType": "video/quicktime"
}
```

## 9.2 `transcript.md`
Purpose:
Human-readable transcript.

Suggested sections:
- source item
- transcript text
- optional timestamps

## 9.3 `transcript.json`
Purpose:
Structured transcript for later processing.

Suggested fields:
- segments
- timestamps
- confidence if available

## 9.4 `story-analysis.json`
Purpose:
Machine-usable summary of why an item may matter.

Suggested shape:

```json
{
  "itemId": "item_2026-04-13_iphone_video_001",
  "summary": "Nate records an early morning reflection before shift about trying to keep building despite fatigue.",
  "themes": ["build-life", "fatigue", "transition"],
  "type": "quote",
  "storyworthy": true,
  "hookIdeas": [
    "Trying to build before a kitchen shift starts.",
    "What founder work looks like when you are exhausted."
  ],
  "bestQuotes": [
    "..."
  ]
}
```

## 9.5 `notes.md`
Purpose:
Human-readable overview of a candidate story.

Contents:
- what happened
- why it matters
- what thread it belongs to

## 9.6 `beats.md`
Purpose:
Ordered list of moments or turns.

## 9.7 `selects.md`
Purpose:
Best clips, best quotes, strongest support material.

## 9.8 `sequence.md`
Purpose:
Suggested narrative order.

## 9.9 `story.json`
Purpose:
Structured story candidate metadata.

Suggested shape:

```json
{
  "storyId": "2026-04-13_building-before-shift",
  "title": "Building Before Shift",
  "themes": ["build-life", "fatigue", "small-win"],
  "status": "candidate",
  "items": [
    "item_2026-04-13_iphone_video_001",
    "item_2026-04-13_plaud_audio_001"
  ],
  "reviewState": "needs-review"
}
```

---

# 10. Naming conventions

## Items
Use stable internal IDs:
- `item_YYYY-MM-DD_source_type_###`

## Stories
Use date + slug:
- `YYYY-MM-DD_slug`

## Exports
Use suffixes for stage/version:
- `vertical-cut-v1.mp4`
- `story-packet-v2.md`

Avoid fancy naming schemes.
Simple names survive.

---

# 11. Retention rules

## Raw material
Keep originals unless storage becomes a real problem.

## Derived files
Can be regenerated, but keep them if regeneration is costly.

## Candidate packets
Keep even if unused.
They may matter later.

## Final exports
Always keep and version clearly.

---

# 12. v1 recommendation

For v1, keep this minimal:
- `inbox/`
- `processing/items/<item-id>/`
- `candidates/<date-slug>/packet/`
- `archive/raw/`
- `logs/`

Do not build all folders just because we can.
Build the ones the workflow actually uses first.

---

# 13. Success condition

This structure is working if:
- raw media is easy to drop in
- each item has one obvious home
- candidate stories are easy to browse
- a human can review packets without getting lost
- future automation has clear, boring file paths

That’s the job.
