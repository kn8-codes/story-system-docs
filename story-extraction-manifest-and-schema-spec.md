# Story Extraction Manifest and Schema Spec

## Purpose

Define exact file shapes for the records the story extraction system will write and read in v1.

This spec covers:
- item manifest
- story packet record
- candidate story metadata (`story.json`)

These shapes are designed to be precise enough to code against while staying readable.

---

# 1. Item manifest

## File path

```text
processing/items/<item-id>/metadata/manifest.json
```

## Purpose

One manifest per ingested media file.
This is the canonical record for:
- identity
- source
- processing state
- transcript state
- extraction state
- grouping relationships

## Required fields

```json
{
  "schemaVersion": "1.0",
  "itemId": "item_2026-04-13_iphone_video_001",
  "sourceFileName": "IMG_4821.MOV",
  "normalizedFileName": "2026-04-13_iphone_video_001.mov",
  "sourceRelativePath": "inbox/2026-04-13/IMG_4821.MOV",
  "processingRelativePath": "processing/items/item_2026-04-13_iphone_video_001",
  "sourceDevice": "iphone",
  "mediaType": "video",
  "mimeType": "video/quicktime",
  "capturedAt": "2026-04-13T05:12:00-04:00",
  "ingestedAt": "2026-04-13T05:28:11-04:00",
  "fileSizeBytes": 18273642,
  "durationSeconds": 42.7,
  "status": {
    "ingest": "complete",
    "transcription": "pending",
    "extraction": "pending",
    "grouping": "ungrouped"
  }
}
```

## Full field spec

### Identity
- `schemaVersion` string, required
- `itemId` string, required
- `sourceFileName` string, required
- `normalizedFileName` string, required
- `sourceRelativePath` string, required
- `processingRelativePath` string, required

### Source
- `sourceDevice` string, required
  - expected values: `iphone`, `ipad`, `plaud`, `m4`, `mbp`, `i7`, `unknown`
- `mediaType` string, required
  - expected values: `video`, `audio`, `image`, `screenshot`, `screenrecord`, `text`, `unknown`
- `mimeType` string, required

### Timing and size
- `capturedAt` string or null, required
- `ingestedAt` string, required
- `fileSizeBytes` number, required
- `durationSeconds` number or null, required

### Status object
- `status.ingest` string, required
  - values: `pending`, `complete`, `failed`
- `status.transcription` string, required
  - values: `pending`, `complete`, `failed`, `not-applicable`
- `status.extraction` string, required
  - values: `pending`, `complete`, `failed`
- `status.grouping` string, required
  - values: `ungrouped`, `candidate`, `archived`, `discarded`

### Optional metadata block

```json
"metadata": {
  "width": 1920,
  "height": 1080,
  "frameRate": 30,
  "audioChannels": 2,
  "hasAudio": true,
  "gps": null,
  "cameraModel": "iPhone 13",
  "notes": null
}
```

### Optional transcript block

```json
"transcription": {
  "eligible": true,
  "engine": "whisper",
  "language": "en",
  "completedAt": null,
  "relativeMarkdownPath": null,
  "relativeJsonPath": null,
  "error": null
}
```

### Optional extraction block

```json
"extraction": {
  "completedAt": null,
  "relativeSummaryPath": null,
  "relativeAnalysisPath": null,
  "typeLabel": null,
  "themes": [],
  "storyworthy": null,
  "error": null
}
```

### Optional grouping block

```json
"grouping": {
  "candidateStoryIds": [],
  "primaryCandidateStoryId": null,
  "groupedAt": null
}
```

## Example complete manifest

```json
{
  "schemaVersion": "1.0",
  "itemId": "item_2026-04-13_iphone_video_001",
  "sourceFileName": "IMG_4821.MOV",
  "normalizedFileName": "2026-04-13_iphone_video_001.mov",
  "sourceRelativePath": "inbox/2026-04-13/IMG_4821.MOV",
  "processingRelativePath": "processing/items/item_2026-04-13_iphone_video_001",
  "sourceDevice": "iphone",
  "mediaType": "video",
  "mimeType": "video/quicktime",
  "capturedAt": "2026-04-13T05:12:00-04:00",
  "ingestedAt": "2026-04-13T05:28:11-04:00",
  "fileSizeBytes": 18273642,
  "durationSeconds": 42.7,
  "status": {
    "ingest": "complete",
    "transcription": "complete",
    "extraction": "complete",
    "grouping": "candidate"
  },
  "metadata": {
    "width": 1920,
    "height": 1080,
    "frameRate": 30,
    "audioChannels": 2,
    "hasAudio": true,
    "gps": null,
    "cameraModel": "iPhone 13",
    "notes": null
  },
  "transcription": {
    "eligible": true,
    "engine": "whisper",
    "language": "en",
    "completedAt": "2026-04-13T05:32:02-04:00",
    "relativeMarkdownPath": "transcripts/transcript.md",
    "relativeJsonPath": "transcripts/transcript.json",
    "error": null
  },
  "extraction": {
    "completedAt": "2026-04-13T05:36:40-04:00",
    "relativeSummaryPath": "analysis/summary.md",
    "relativeAnalysisPath": "analysis/story-analysis.json",
    "typeLabel": "quote",
    "themes": ["build-life", "fatigue", "transition"],
    "storyworthy": true,
    "error": null
  },
  "grouping": {
    "candidateStoryIds": ["2026-04-13_building-before-shift"],
    "primaryCandidateStoryId": "2026-04-13_building-before-shift",
    "groupedAt": "2026-04-13T05:40:10-04:00"
  }
}
```

---

# 2. Story packet record

## Purpose

This is the human-readable packet set for one candidate story.
It is stored as markdown files inside the candidate story folder.

## File paths

```text
candidates/<story-id>/packet/notes.md
candidates/<story-id>/packet/beats.md
candidates/<story-id>/packet/selects.md
candidates/<story-id>/packet/sequence.md
candidates/<story-id>/packet/hook-ideas.md
```

## 2.1 `notes.md`

### Purpose
Explain the story in plain language.

### Template

```md
# <story title>

## Summary
<1-2 paragraph summary of what happened>

## Why it matters
- <point>
- <point>

## Main themes
- <theme>
- <theme>

## Current status
- candidate
- needs review

## Next action
- <next action>
```

### Required sections
- `# <story title>`
- `## Summary`
- `## Why it matters`
- `## Main themes`
- `## Current status`
- `## Next action`

## 2.2 `beats.md`

### Purpose
List the key moments or turns in order.

### Template

```md
# Beats

1. <opening beat>
2. <next beat>
3. <turn>
4. <resolution or open loop>
```

### Required sections
- `# Beats`
- ordered list of beats

## 2.3 `selects.md`

### Purpose
Track the strongest clips, quotes, and useful supporting material.

### Template

```md
# Selects

## Best clips
- `<item-id>` — <why it matters>
- `<item-id>` — <why it matters>

## Best quotes
- "<quote>" — `<item-id>`
- "<quote>" — `<item-id>`

## Useful support material
- `<item-id>` — <b-roll / texture / transition use>
```

### Required sections
- `# Selects`
- `## Best clips`
- `## Best quotes`

## 2.4 `sequence.md`

### Purpose
Propose a rough narrative order.

### Template

```md
# Sequence

## Proposed order
1. <opening>
2. <build>
3. <turn>
4. <close>

## Notes
- <note>
- <note>
```

### Required sections
- `# Sequence`
- `## Proposed order`

## 2.5 `hook-ideas.md`

### Purpose
Generate possible openings or frames.

### Template

```md
# Hook Ideas

- <hook>
- <hook>
- <hook>
```

### Required sections
- `# Hook Ideas`
- bullet list of hooks

---

# 3. Candidate story metadata (`story.json`)

## File path

```text
candidates/<story-id>/metadata/story.json
```

## Purpose

Canonical machine-readable record for one candidate story.
This ties together:
- identity
- story status
- included items
- themes
- packet state
- review state

## Required fields

```json
{
  "schemaVersion": "1.0",
  "storyId": "2026-04-13_building-before-shift",
  "title": "Building Before Shift",
  "slug": "building-before-shift",
  "createdAt": "2026-04-13T05:40:10-04:00",
  "updatedAt": "2026-04-13T05:48:00-04:00",
  "status": "candidate",
  "reviewState": "needs-review",
  "themes": ["build-life", "fatigue", "small-win"],
  "itemIds": [
    "item_2026-04-13_iphone_video_001",
    "item_2026-04-13_plaud_audio_001"
  ]
}
```

## Full field spec

### Identity
- `schemaVersion` string, required
- `storyId` string, required
- `title` string, required
- `slug` string, required
- `createdAt` string, required
- `updatedAt` string, required

### State
- `status` string, required
  - values: `candidate`, `promoted`, `archived`, `discarded`
- `reviewState` string, required
  - values: `needs-review`, `reviewed`, `promote-to-edit`, `archive`, `ignored`

### Content
- `themes` array of strings, required
- `itemIds` array of strings, required
- `primaryItemId` string or null, optional
- `summary` string or null, optional
- `whyItMatters` string or null, optional

### Packet paths

```json
"packet": {
  "notesPath": "packet/notes.md",
  "beatsPath": "packet/beats.md",
  "selectsPath": "packet/selects.md",
  "sequencePath": "packet/sequence.md",
  "hookIdeasPath": "packet/hook-ideas.md"
}
```

### Transcript/reference paths

```json
"references": {
  "combinedTranscriptPath": "transcripts/combined-transcript.md",
  "mediaRelativePaths": [
    "media/2026-04-13_iphone_video_001.mov"
  ]
}
```

### Review metadata

```json
"review": {
  "reviewedAt": null,
  "reviewedBy": null,
  "nextAction": "review-packet",
  "notes": null
}
```

### Optional scoring block

```json
"score": {
  "storyStrength": 0.78,
  "confidence": 0.66
}
```

## Example complete `story.json`

```json
{
  "schemaVersion": "1.0",
  "storyId": "2026-04-13_building-before-shift",
  "title": "Building Before Shift",
  "slug": "building-before-shift",
  "createdAt": "2026-04-13T05:40:10-04:00",
  "updatedAt": "2026-04-13T05:48:00-04:00",
  "status": "candidate",
  "reviewState": "needs-review",
  "themes": ["build-life", "fatigue", "small-win"],
  "itemIds": [
    "item_2026-04-13_iphone_video_001",
    "item_2026-04-13_plaud_audio_001"
  ],
  "primaryItemId": "item_2026-04-13_iphone_video_001",
  "summary": "An early morning reflection about trying to build before the kitchen shift starts.",
  "whyItMatters": "It captures the actual tension between ambition, fatigue, and limited time.",
  "packet": {
    "notesPath": "packet/notes.md",
    "beatsPath": "packet/beats.md",
    "selectsPath": "packet/selects.md",
    "sequencePath": "packet/sequence.md",
    "hookIdeasPath": "packet/hook-ideas.md"
  },
  "references": {
    "combinedTranscriptPath": "transcripts/combined-transcript.md",
    "mediaRelativePaths": [
      "media/2026-04-13_iphone_video_001.mov",
      "media/2026-04-13_plaud_audio_001.m4a"
    ]
  },
  "review": {
    "reviewedAt": null,
    "reviewedBy": null,
    "nextAction": "review-packet",
    "notes": null
  },
  "score": {
    "storyStrength": 0.78,
    "confidence": 0.66
  }
}
```

---

# 4. Validation rules

## Item manifest rules
- `itemId` must be unique
- `status` fields must use allowed values
- `themes` must be strings
- `candidateStoryIds` must refer to story IDs when present

## Story metadata rules
- `storyId` must be unique within `candidates/`
- `itemIds` must point to real item manifests
- `packet` paths must be relative paths
- `reviewState` must use allowed values

## Markdown packet rules
- required headings must exist
- packet files must be UTF-8 markdown
- packet file names should stay stable

---

# 5. Minimum v1 requirement

To count as a working v1 data model, the system must be able to produce:
- one valid `manifest.json` per ingested file
- one valid `story.json` per candidate story
- one packet set with `notes.md`, `beats.md`, `selects.md`, `sequence.md`, and `hook-ideas.md`

That is enough to write code against.
