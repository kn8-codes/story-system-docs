# Story Extraction v1 Implementation Checklist

## Goal

Turn the story extraction system from docs into an executable v1 build plan.

Each task should be small enough to finish in one sitting.
Each milestone should produce something testable.

Core promise:

**Drop media in. Get back a story packet.**

---

# Milestone 1 — Inbox setup

## Outcome

A usable inbox exists and raw media can be dropped in without confusion.

## Tasks

### 1.1 Create the top-level folder structure
Create:

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

Definition of done:
- folders exist
- paths are documented in one place

### 1.2 Create date-based inbox convention
Decide that inbox drops go into:

```text
inbox/YYYY-MM-DD/
```

Definition of done:
- one example date folder exists
- naming rule is documented in `config/naming-rules.md`

### 1.3 Write `config/naming-rules.md`
Document:
- source naming pattern
- normalized item naming pattern
- story folder slug rules
- export version naming

Definition of done:
- naming rules exist in markdown
- examples are included

### 1.4 Create starter config files
Create:

```text
config/tags.json
config/themes.json
config/workflow.json
```

Definition of done:
- files exist with placeholder content
- basic theme/tag values are seeded

### 1.5 Add sample raw media folder contents
Put 2 to 5 realistic sample files into a dated inbox folder.

Definition of done:
- sample inputs exist
- filenames are messy enough to test normalization

## First script needed

### `scripts/bootstrap_story_system.py`
Purpose:
- create the folder structure
- create starter config files if missing
- optionally create today’s inbox folder

Inputs:
- base path

Outputs:
- directory tree
- starter config files

---

# Milestone 2 — Manifest

## Outcome

Every ingested media file gets a stable internal identity and a machine-readable record.

## Tasks

### 2.1 Define item ID format
Adopt:
- `item_YYYY-MM-DD_source_type_###`

Definition of done:
- documented in code and config

### 2.2 Write manifest schema file
Create a machine-readable schema reference for item manifests.

Definition of done:
- field list exists
- required vs optional fields are clear

### 2.3 Write intake/normalization script
Script scans `inbox/YYYY-MM-DD/`, creates one processing folder per item, and generates initial manifest data.

Definition of done:
- one processing folder created per item
- each item gets `metadata/manifest.json`

### 2.4 Normalize filenames during intake
If filenames are ugly or inconsistent, create normalized copies or move to normalized names in processing.

Definition of done:
- every item has a normalized source filename in its processing folder

### 2.5 Write append-only intake log
Record what was ingested and when.

Definition of done:
- `logs/intake/YYYY-MM-DD.log` created
- each ingested item gets one log entry

## First scripts needed

### `scripts/intake_inbox.py`
Purpose:
- scan inbox date folder
- create item IDs
- build `processing/items/<item-id>/`
- copy or move source file into `source/`
- create initial `metadata/manifest.json`
- append log entry

Inputs:
- inbox folder path

Outputs:
- processing item folders
- manifest files
- intake log entries

### `scripts/normalize_filename.py`
Purpose:
- generate normalized filenames from source metadata + conventions

Inputs:
- source filename
- source device hint if available
- media type
- date

Outputs:
- normalized filename string

---

# Milestone 3 — Transcription

## Outcome

Audio/video items can produce transcripts automatically.

## Tasks

### 3.1 Define which media types get transcription
At minimum:
- audio
- video with audio track

Definition of done:
- transcription eligibility rule documented

### 3.2 Write transcription runner script
Run Whisper or chosen transcription backend against eligible items.

Definition of done:
- transcript markdown exists for one test item
- transcript JSON exists for one test item

### 3.3 Update manifest with transcription status
Add fields such as:
- `transcription.status`
- `transcription.engine`
- `transcription.completedAt`

Definition of done:
- manifest changes after transcript finishes

### 3.4 Add transcription logs
Append success/failure to daily transcription log.

Definition of done:
- `logs/transcription/YYYY-MM-DD.log` exists

## First script needed

### `scripts/transcribe_items.py`
Purpose:
- find eligible items in processing
- run transcription
- write `transcripts/transcript.md`
- write `transcripts/transcript.json`
- update manifest
- log result

Inputs:
- base processing path
- optional item ID filter

Outputs:
- transcript files
- updated manifest fields
- transcription log entries

---

# Milestone 4 — Item extraction

## Outcome

Each media item gets a usable summary, tags, and story analysis.

## Tasks

### 4.1 Define item-level extraction prompt/spec
Document what the extraction step must output:
- summary
- themes
- type label
- best quotes
- hook ideas
- storyworthiness

Definition of done:
- prompt or extraction template exists in `config/prompts/`

### 4.2 Write item extraction script
Run extraction on one item at a time using manifest + transcript + metadata.

Definition of done:
- `analysis/summary.md`
- `analysis/story-analysis.json`
- manifest updated with summary fields

### 4.3 Add type and theme validation
Make sure outputs only use allowed type/theme values when possible.

Definition of done:
- bad tags get normalized or flagged

### 4.4 Add extraction logs
Write success/failure to `logs/extraction/YYYY-MM-DD.log`.

Definition of done:
- daily extraction log exists

## First scripts needed

### `scripts/extract_story_from_item.py`
Purpose:
- read manifest + transcript + metadata
- generate item-level story analysis
- write `summary.md`
- write `story-analysis.json`
- update manifest fields

Inputs:
- item ID or item folder path

Outputs:
- analysis files
- updated manifest
- extraction log entry

### `scripts/validate_tags.py`
Purpose:
- validate theme and type labels against config
- normalize obvious drift

Inputs:
- `story-analysis.json`
- `config/tags.json`
- `config/themes.json`

Outputs:
- cleaned or flagged labels

---

# Milestone 5 — Candidate grouping

## Outcome

Related items get grouped into candidate story folders.

## Tasks

### 5.1 Define grouping heuristics
Use simple v1 rules:
- same day proximity
- overlapping themes
- repeated entities/projects
- shared transcript language

Definition of done:
- grouping rules documented in markdown or config

### 5.2 Write grouping script
Scan processed items and create candidate story folders.

Definition of done:
- at least one candidate folder generated from test items

### 5.3 Generate candidate story metadata
Write `metadata/story.json` for each candidate.

Definition of done:
- candidate folder includes complete story metadata file

### 5.4 Combine transcripts and references
Create a merged transcript or reference summary inside the candidate folder.

Definition of done:
- `transcripts/combined-transcript.md` exists when applicable

### 5.5 Log grouping events
Append candidate creation/update events to `logs/grouping/YYYY-MM-DD.log`.

Definition of done:
- grouping log exists

## First script needed

### `scripts/group_items_into_candidates.py`
Purpose:
- scan processed item analyses
- identify likely related items
- create `candidates/YYYY-MM-DD_slug/`
- copy/reference media
- write `metadata/story.json`
- write combined transcript reference

Inputs:
- processing path
- optional date scope

Outputs:
- candidate folders
- story metadata files
- grouping logs

---

# Milestone 6 — Packet generation

## Outcome

Each candidate story gets a reviewable packet.

## Tasks

### 6.1 Define packet sections
At minimum:
- notes
- beats
- selects
- sequence
- hook ideas

Definition of done:
- packet template exists

### 6.2 Write packet generator script
Read candidate metadata and item analyses, then generate markdown packet files.

Definition of done:
- `packet/notes.md`
- `packet/beats.md`
- `packet/selects.md`
- `packet/sequence.md`
- `packet/hook-ideas.md`

### 6.3 Add packet review state
Track whether a packet is:
- `needs-review`
- `reviewed`
- `promote-to-edit`
- `archive`

Definition of done:
- `story.json` includes review state

### 6.4 Log packet generation
Append to `logs/exports/` or dedicated packet log.

Definition of done:
- packet generation leaves a trace in logs

## First script needed

### `scripts/generate_story_packet.py`
Purpose:
- read candidate folder contents
- generate human-readable packet markdown files
- update story metadata review state if needed

Inputs:
- story folder or story ID

Outputs:
- packet markdown files
- updated story metadata
- packet generation log entry

---

# Milestone 7 — Human review loop

## Outcome

The system is usable by Nate without turning into another black box.

## Tasks

### 7.1 Define review workflow
For each packet, Nate should be able to choose:
- keep
- merge
- ignore
- promote to rough cut
- archive

Definition of done:
- one markdown doc explains review actions

### 7.2 Add review status fields to story metadata
Track:
- review state
- reviewed at
- reviewed by
- next action

Definition of done:
- `story.json` supports review state changes

### 7.3 Add manual notes field
Allow story-level human notes without breaking automation.

Definition of done:
- notes can be added without ambiguity

## First script needed

### `scripts/update_story_review_state.py`
Purpose:
- update `story.json` with review state and next action

Inputs:
- story ID
- new state
- optional note

Outputs:
- updated story metadata
- log entry

---

# Recommended first build order

## Order that makes sense
1. `bootstrap_story_system.py`
2. `intake_inbox.py`
3. `transcribe_items.py`
4. `extract_story_from_item.py`
5. `group_items_into_candidates.py`
6. `generate_story_packet.py`
7. `update_story_review_state.py`

That is the minimum real machine.

---

# Suggested v1 milestone checkpoints

## Checkpoint A
Can raw media be dropped into inbox and turned into item folders with manifests?

## Checkpoint B
Can an item produce a transcript and story analysis?

## Checkpoint C
Can multiple items be grouped into one candidate story?

## Checkpoint D
Can a candidate produce a readable story packet?

## Checkpoint E
Can Nate review that packet and decide what to do next?

If all five are true, v1 is real.
