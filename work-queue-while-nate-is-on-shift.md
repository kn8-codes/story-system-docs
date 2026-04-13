# Work Queue While Nate Is On Shift

## Goal

Create a clear list of things kn8bot can work on independently without needing Nate glued to the terminal.

This should prioritize:
- low-risk work
- docs/specs/structure that create leverage
- repo-safe work
- project prep that reduces friction later

Not included:
- destructive changes
- paid API actions without review
- DB changes without explicit go-ahead

---

# 1. HOSP or Not

## Current status
Strongest progress right now.
Backend migration is live in the correct Supabase project.
Helper RPCs and duplicate prevention were manually verified.
Frontend cutover still needs visual/UI testing and eventual direct-insert hardening.

## Good independent tasks

### A. Frontend RPC cutover audit
What I can do:
- review repo again file by file
- check for any lingering direct inserts to `votes`
- make a precise cutover checklist

### B. Blurb/update path verification plan
What I can do:
- write exact expected behavior doc
- inspect code paths for edge cases
- identify where realtime or UI mismatch might happen

### C. Post-cutover hardening checklist
What I can do:
- prep exact runbook for when to apply `supabase-post-cutover-direct-insert-block.sql`
- define success/failure conditions clearly

### D. Landing page refinement doc
What I can do:
- tighten copy/CTA guidance further
- create a visual hierarchy checklist
- suggest mobile QA cases

### E. Shareability plan
What I can do:
- spec shareable case pages
- OG image ideas
- outcome-post content structure

## Best HOSP task to do next solo
**frontend RPC cutover audit + hardening checklist**

---

# 2. Story Extraction System

## Current status
Three solid docs are now written:
- processing flow
- folder/file spec
- toolchain map

## Good independent tasks

### A. v1 implementation checklist
Turn the docs into:
- concrete tasks
- milestones
- first scripts needed

### B. manifest/schema spec
Define exact JSON/markdown fields for:
- item manifests
- story packet records
- candidate story metadata

### C. pipeline pseudocode
Write the actual step logic:
- ingest
- normalize
- transcribe
- summarize
- group
- packetize

### D. MBP watch-folder architecture
Write a clean v1 plan for:
- local processing host behavior
- folder watchers
- output placement

## Best story-system task to do next solo
**v1 implementation checklist + manifest spec**

---

# 3. OBDsidian

## Current status
Per memory and AGENTS:
- repo reset needed before meaningful dev
- do not drift into feature work before reset

## Good independent tasks

### A. Repo reset checklist
What I can do:
- define exact reset workflow
- identify what must be preserved
- identify what should be discarded
- prep backup/verification list

### B. Sync problem diagnosis plan
What I can do:
- write likely root causes between M4 and i7 workflows
- define clean resync strategy

### C. Environment inventory template
What I can do:
- create a machine-by-machine dev environment checklist
- make future cross-machine work less messy

## Best OBDsidian task to do next solo
**repo reset checklist**

---

# 4. OLA Arbitrage

## Current status
Frontend exists.
Next real build task is Python scraper backend.
Good candidate for structured prep work.

## Good independent tasks

### A. Scraper architecture spec
What I can do:
- define inputs/outputs
- define MAC.BID scrape flow
- define eBay comp lookup flow
- define scoring pipeline steps

### B. Data model draft
What I can do:
- define lot record shape
- comp record shape
- scoring output shape
- storage/export format

### C. Failure and rate-limit plan
What I can do:
- identify anti-bot / drift risks
- define retries and cache strategy

### D. CLI or job-runner spec
What I can do:
- define how scraper should be run
- one-shot vs batch vs scheduled

## Best OLA task to do next solo
**scraper architecture spec + data model draft**

---

# 5. BoomMates

## Current status
On hold until Civic Assembly winds down.
But planning and synthesis work can still happen safely.

## Good independent tasks

### A. Research structure cleanup
What I can do:
- propose foldering / knowledge structure
- identify core research lanes

### B. Problem framing doc
What I can do:
- tighten the thesis
- define target users more sharply
- clarify initial wedge

### C. Civic-to-product bridge notes
What I can do:
- map assembly ideas to actual product opportunities

## Best BoomMates task to do next solo
**civic-to-product bridge notes**

---

# 6. OpenClaw / multi-machine ops

## Current status
Important future infra idea captured:
- install OpenClaw on the other Macs
- use CodexBar/local usage monitoring
- export monitored usage paths to NAS

## Good independent tasks

### A. Multi-machine architecture note
What I can do:
- define machine roles
- define what logs/artifacts sync to NAS
- define minimal first version

### B. Usage export format
What I can do:
- define a daily snapshot format for each machine
- prepare aggregation ideas

### C. Deployment checklist per machine
What I can do:
- M4
- MBP
- i7

## Best OpenClaw task to do next solo
**multi-machine architecture note**

---

# 7. Priority order for independent work

If Nate is on shift and I’m picking the highest-leverage solo work, I should probably go:

1. HOSP frontend RPC cutover audit + hardening checklist
2. Story extraction v1 implementation checklist + manifest spec
3. OBDsidian repo reset checklist
4. OLA scraper architecture spec
5. OpenClaw multi-machine architecture note
6. BoomMates civic-to-product bridge notes

---

# 8. Recommendation

Best immediate next task while Nate works:

**Do the HOSP frontend RPC cutover audit and produce a no-guesswork checklist for final hardening.**

Why:
- the backend is already alive
- the finish line is visible
- it reduces risk before the final cutover step
- it keeps momentum on the hottest project thread
