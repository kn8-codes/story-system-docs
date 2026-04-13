# Story Extraction Toolchain Map

## Goal

Map the actual machines and tools into a working story system.

This is the practical layer:
- what each machine does
- what each tool is for
- how files move
- where OpenClaw fits
- where NAS fits

The point is not to create a fancy distributed system.
The point is to create a durable, low-friction storytelling machine.

---

# 1. System roles by device

## iPhone
### Role
Primary capture device.

### Best uses
- short clips
- photos
- in-the-moment footage
- quick voice notes
- environmental texture
- real-life scenes as they happen

### Strengths
- always present
- lowest friction capture
- best for catching the actual moment

### Weaknesses
- not a good place to process or organize deeply

---

## iPad
### Role
Review and lightweight intake interface.

### Best uses
- reviewing footage
- reading story packets
- making quick notes
- moving files around in Files/iCloud
- rough triage while away from desk

### Strengths
- bigger than phone
- easier review surface
- couch/travel-friendly

### Weaknesses
- not the main processing or editing machine

---

## i7 Air (Jeep machine)
### Role
Field machine and lightweight operator node.

### Best uses
- quick reviews
- lightweight note cleanup
- repo/docs work
- emergency edits
- intake verification

### Strengths
- mobile build station
- works from the Jeep / in-between spaces

### Weaknesses
- monitor constraints
- not ideal for heavy media processing

### Recommendation
Use it for:
- ideas
- review
- orchestration
- not the heavy lifting

---

## M4 Air
### Role
Primary operator machine.

### Best uses
- planning the system
- reviewing packets
- orchestrating OpenClaw flows
- light scripting and pipeline development
- managing docs, prompts, structure, metadata logic

### Strengths
- best all-around daily driver
- best place to think and steer

### Weaknesses
- should not become the machine doing every heavy media job if MBP can take that load

---

## M1 MBP
### Role
Story shop and processing engine.

### Best uses
- ingest worker
- transcription runs
- ffmpeg transforms
- contact sheet generation
- rough cut assembly
- export rendering
- watch-folder automation

### Strengths
- best suited for sustained heavier media work
- can be left doing batch work

### Weaknesses
- less ideal as the main control tower if M4 is the primary machine

### Recommendation
This should become the actual backend of the story system.

---

## NAS
### Role
Archive and shared storage backbone.

### Best uses
- raw archive
- processed archive
- exports store
- cross-machine drop location
- logs/manifests backup

### Strengths
- centralized storage
- machine-agnostic persistence

### Weaknesses
- should not become the place where logic lives first

### Recommendation
Use NAS as storage + durability, not as the first place complexity gets piled.

---

# 2. Functional layers

## Layer A — Capture
Devices:
- iPhone
- iPad
- Plaud
- occasional Mac screen capture

Goal:
- catch moments fast

## Layer B — Intake
Devices:
- M4 Air
- i7 Air
- iPad lightly

Goal:
- move material into inbox cleanly

## Layer C — Processing
Devices:
- M1 MBP primarily

Goal:
- transcribe
- extract metadata
- create thumbnails
- run story extraction
- generate packets

## Layer D — Review
Devices:
- M4 Air
- iPad
- i7 Air lightly

Goal:
- decide what matters
- group stories
- review packet quality

## Layer E — Archive and export
Devices:
- NAS
- MBP

Goal:
- preserve work
- store exports
- keep history durable

---

# 3. Tool map by job

## Ingest / file movement
### Likely tools
- Finder / Files for manual drops
- rsync later for reliable sync
- folder watch scripts later

### v1 recommendation
Keep ingest simple and mostly manual at first.
Do not over-automate the first mile.

---

## Transcription
### Likely tools
- Whisper CLI / local Whisper
- Plaud export text when available
- OpenClaw `openai-whisper` skill later if useful

### Recommendation
Transcription should run on the MBP.
That is a perfect batch-processing task.

---

## Media transforms
### Likely tools
- ffmpeg
- contact sheet / thumbnail scripts
- frame extraction tools later

### Recommendation
Also MBP territory.
Do not waste the i7 on that unless forced.

---

## Story extraction / packet drafting
### Likely tools
- OpenClaw for orchestration
- markdown templates
- prompt files in config
- maybe local scripts calling models or local summarizers later

### Recommendation
This is where OpenClaw actually shines.
OpenClaw should not be the renderer.
It should be the mechanic coordinating:
- transcript in
- summary out
- packet generated
- candidate grouped

---

## Review
### Likely tools
- markdown files
- Obsidian later if wanted
- Files app / editor on iPad
- M4 as primary review surface

### Recommendation
Keep outputs in plain markdown so they survive every device.

---

# 4. Where OpenClaw fits

## OpenClaw should do
- orchestrate processing steps
- generate summaries
- tag and classify items
- draft story packets
- update logs/manifests
- help route work between machines later

## OpenClaw should NOT be asked to do
- heavy media rendering itself
- be the permanent storage layer
- replace human judgment about what is true or worth sharing

## Best frame
OpenClaw is the workflow mechanic.
The MBP is the machine shop.
The NAS is the warehouse.
The phone is the camera in your pocket.

That split actually makes sense.

---

# 5. Where Codex/OpenAI usage fits

## Good use of model calls
- summarizing transcripts
- extracting themes
- identifying quotes
- drafting packet text
- grouping media into candidate stories

## Bad use of model calls
- constant frame-by-frame video analysis by default
- using tokens for things ffmpeg can do locally
- turning every low-value media item into an expensive multimodal job

## Recommendation
Let local tools handle the heavy media labor.
Let models handle language and meaning.
That keeps the system sane.

---

# 6. NAS integration plan

## v1
NAS is just:
- backup/archive destination
- optional shared exports location

## v2
NAS can become:
- raw archive mirror
- processed output mirror
- multi-machine inbox drop

## v3
NAS can support:
- per-machine usage exports
- automated manifests
- shared story archive

### Important
Do not make the NAS a hard dependency for v1 usability.
The system should still function locally if NAS is unavailable.

---

# 7. Recommended v1 deployment model

## Capture
Phone / Plaud / screenshots

## Intake
Drop into `story-system/inbox/`

## Processing host
M1 MBP

## Review host
M4 Air

## Archive destination
NAS later, local disk first if needed

That is the simplest sane setup.

---

# 8. Future automation opportunities

Later, once the basics work:
- watch-folder on MBP
- automatic Whisper transcription jobs
- automatic thumbnail/contact sheet creation
- automatic packet generation
- scheduled archive sync to NAS
- cross-machine intake manifests

But not before the simple version proves useful.

---

# 9. Risks to avoid

## Risk 1
Overbuilding distributed infrastructure before the story packets are even useful.

## Risk 2
Letting the MBP become a mysterious black box nobody trusts.

## Risk 3
Making the pipeline depend on internet/model access for every tiny step.

## Risk 4
Building a media machine that helps process files but not tell better stories.

---

# 10. Recommendation

Use this machine split:

- **iPhone** = capture
- **iPad** = review on the move
- **i7 Air** = field ops / light review
- **M4 Air** = control tower
- **M1 MBP** = processing engine
- **NAS** = archive backbone
- **OpenClaw** = workflow mechanic

That is the cleanest version of the system.
