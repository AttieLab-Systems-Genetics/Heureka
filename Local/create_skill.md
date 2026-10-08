# Team skill: `attie-data` — Attie Lab data locator

## Project Rationale

### Goal

Develop a team-level skill that shows where to find Attie data.

### Background

Attie data is in several locations and is rather complicated.

### Approach

I have organized much of data description in `https://github.com/AttieLab-Systems-Genetics/AttieLabData` and in repos linked through this.  Set up skill that either ingest these repos or points to them in a useful way.

### Success Criteria

Attie team can find data and access it quickly without duplication of effort or multiple data copies.

### Prompts

- Create skill as described in project rationale.
- Where do I find the products of this latest activity?

## What was built

A new workspace-scoped skill at `~/Heureka Bench/.heureka/skills/attie-data/` (visible to every project in your Bench workspace, so the whole team benefits):

| File | Purpose |
| --- | --- |
| `SKILL.md` (72 lines) | Dispatch logic: when to activate, required answer format, and a refresh protocol |
| `data_map.md` (115 lines) | The complete data map — storage units, folder layouts, naming conventions, formats, repo table, app endpoints |

I chose workspace scope over project scope deliberately: "where is X?" questions come up in any analysis project, and the rationale's success criterion (team finds data quickly, no duplicate copies) requires the skill to travel with the team, not this one folder.

## How the skill works

1. **Answer from the snapshot first.** The map is a dated (2026-10-07) distillation of the official docs in `AttieLab-Systems-Genetics/AttieLabData` (`README.md`, `RD_Data.md`, `S3_Data.md`) plus the READMEs and `DEVELOPER.md` of all five linked repos — so one Read answers most "where is it?" questions without re-hitting GitHub.
2. **Refresh protocol for drift.** If a question doesn't match the map, or the snapshot is ~3 months old, the skill instructs a shallow clone of `AttieLabData` and re-read of `README.md`/`RD_Data.md`/`S3_Data.md` plus linked-repo READMEs, then rewrites `data_map.md` in place with a new compiled date. The skill stays current as the lab's layout evolves.
3. **Standardized answer format.** Every location answer must give: (1) exact storage unit + path, (2) access method (RD mount / Globus endpoint / RStudio Connect, with UW-credential note), (3) documenting repo, (4) format & scale incl. paired `*_rows.fst` indexes, and (5) the **no-copy rule** — point to the canonical location, prefer streaming/DuckDB-over-S3, and if a local copy is genuinely needed, place it in the project's `Data/` with the source path recorded in a header comment.

## What the data map covers (verified live today)

- **3 storage units**: ResearchDrive `adattie` (legacy, near full), ResearchDrive `mkeller3` (current), S3 `adattie-bucket-01` (`/2026/`, collection `wisc-s3-campus-dev`, Globus endpoint id included).
- **DO1200** at `mkeller3/General/main_directory` → documented in `sysgenDO1200` (per-subfolder READMEs, symlinked into RD).
- **Keller workflows** at `mkeller3/General/Projects2` → `mkeller3Projects2` (ComBat/rank-Z, QTL/hotspot, WGCNA, Bayesian mediation).
- **Founder studies** at `adattie/General/founder_*` → `founder_diet_study` repo, with full RawData/HarmonizedData layout and row counts (~23.3M rows; `traitData.sqlite` ~192 MB), plus the Parquet migration schema pointer in `DEVELOPER.md`.
- **S3 2026**: `prod/miniViewer_3.0` (274 `.fst` scan matrices, full naming schema decoded: chr × isoforms/metabolites × all/female/male/HC/HF × additive/diet_interactive/sex_interactive), `dev/DO_mapping_files/output` (53,684 per-gene top-SNP CSVs with naming pattern), and the 7-directory `physiological_qtl_app_s3` backend.
- **Formats guide** (XLSX→CSV→RDS, qtl2fst, sqlite, parquet migration status) and a **repo table** with correct default branches (`main` vs `master`) and public/private access.
- **App endpoints**: FounderDietStudy, FounderCalciumStudy (RStudio Connect), foundrShiny.

## Verification notes

- All repo metadata, READMEs, and the `DEVELOPER.md` were pulled live from GitHub on 2026-10-07 — nothing in the map is from memory.
- One discrepancy caught: `mkeller3Projects2`'s own README claims the repo is private, but the GitHub API reports it public — the map records both facts rather than silently picking one.
- Log entry written to `ExperimentLog` (tagged `skill`, `data-access`, `attie-lab`) and findings appended to project `MEMORY.md`.

## Suggested next steps

1. **Test it**: ask "where are the DO liver metabolite scan files for HF mice?" or "where is the founder diet harmonized transcriptomics data?" — the skill should activate and answer in the standard format.
2. **Open question left in memory**: the founder diet RawData was originally collated from a Google Drive folder (link in the repo). I referenced it but didn't map its structure — worth a section if the team still consults it.
3. Consider mirroring a copy of `data_map.md` into the `AttieLabData` repo itself (as a rendered page) so non-Bench lab members get the same one-stop reference.

The skill will appear in **Settings → ARC → Skills** once the app reloads the workspace skills.

## Where the attie-data Skill Outputs Live

**Question:** Where do I find the products of the attie-data skill work, notably the markdown documents?

**Short answer:** the deliverables are markdown files in the *workspace skill folder*, not in this project:

```
~/Heureka Bench/.heureka/skills/attie-data/
├── SKILL.md      (3.5 KB — the skill instructions)
└── data_map.md   (6.1 KB — the data map snapshot, compiled 2026-10-07)
```

### What each file is

| File | Contents |
| --- | --- |
| `SKILL.md` | The skill body: when to activate, the refresh protocol (~3-month re-clone of `AttieLab-Systems-Genetics/AttieLabData`), the 5-part answer format (location → access → documentation → format & scale → no-copy rule), and a quick-orientation summary. |
| `data_map.md` | The actual data map — a compiled snapshot of the official lab data docs (RD units `adattie`/`mkeller3`, S3 bucket `adattie-bucket-01`, linked repos, default branches, formats). This is the file to read first for any "where is X?" question. |

### Why they're not in this project

The skill was intentionally created **workspace-scoped** (2026-10-07 decision), so every Attie team project under `~/Heureka Bench/` can use it — not just "Skill for Attie Data". Project-scoped skills would have landed in `Skill for Attie Data/.heureka/skills/`.

### How to see them

- **In the app:** Settings → ARC → Skills — the skill appears as `attie-data` (workspace scope). Reference files sit alongside SKILL.md in the skill folder.
- **In Finder/terminal:** `~/Heureka Bench/.heureka/skills/attie-data/`
- **In an ARC session:** just ask a data-location question ("where are the DO liver metabolite scan files?") and the skill activates; you can also read `data_map.md` directly.

### Other artifacts from the activity

- **Project memory:** `Skill for Attie Data/.heureka/MEMORY.md` — 2026-10-07 entry with the key findings (public-vs-private discrepancy on `mkeller3Projects2`, default branches, Parquet migration schema in `founder_diet_study/DEVELOPER.md`) and the open question about the founder-diet Google Drive folder.
- **This project's other folders** (`Analysis/`, `Code/`, `Data/`, `Documents/`, `Experiments/`) are empty except for this answer — no analysis outputs were produced, only the skill.
- The GitHub repos themselves (`AttieLabData`, `sysgenDO1200`, `sysgenAnalysis`, `mkeller3Projects2`, `founder_diet_study`) were read as sources, not cloned into the workspace.

### Next step (from project memory)

Test the skill in a live session with a "where is X?" question and confirm the answer format is useful to the team; iterate on SKILL.md if needed.
