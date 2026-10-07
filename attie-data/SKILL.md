---
name: attie-data
description: Locate Attie Lab data: where datasets live (RD/S3), how to access them, which repo documents them
---

# Attie Lab Data Locator

The user is looking for Attie Lab (UW-Madison systems genetics) data: where a
dataset lives, how to access it, which file/repo documents it, or how to
access it without duplicating copies. Use this skill to answer "where is X?"
questions about Attie lab data and to plan data access.

## First step — always check the data map

Read `data_map.md` (sits alongside this file). It is a snapshot of the
official data documentation in `AttieLab-Systems-Genetics/AttieLabData` and
its linked repos, compiled 2026-10-07. Answer from it.

## When the map may be stale — refresh protocol

The lab's data layout evolves. If the user's question doesn't match the map
(a project, folder, or app not listed), or if it has been ~3 months since the
snapshot date in `data_map.md`, refresh:

```bash
git clone --depth 1 https://github.com/AttieLab-Systems-Genetics/AttieLabData /tmp/AttieLabData
```

Then read `README.md`, `RD_Data.md`, `S3_Data.md`, and any newly added files.
For linked repos, fetch their READMEs from raw.githubusercontent.com (branches:
`main` for sysgenDO1200, sysgenAnalysis; `master` for mkeller3Projects2,
founder_diet_study). If the official docs changed, update `data_map.md` in
place with the new content and a new "compiled" date, and tell the user the
map was refreshed.

## Answer format

When answering "where is <data>?":

1. **Location first**: the exact storage unit and path (RD partition + folder,
   or S3 bucket + prefix, or Globus path).
2. **Access**: how to reach it (RD mount, Globus endpoint link, S3 via Globus,
   RStudio Connect app) — note that RD and S3 require UW credentials.
3. **Documentation**: the GitHub repo/file that documents the contents and
   naming conventions.
4. **Format & scale**: file formats present (fst, RDS, sqlite, CSV, parquet),
   approximate scale, and any paired index files (e.g. `*_rows.fst`).
5. **No-copy rule**: point to the canonical location. If the user needs the
   data locally for analysis, recommend streaming from RD/S3 (or DuckDB
   queries against S3 for parquet) rather than copying large sets; if a copy
   is genuinely needed, put it in the current project's `Data/` and note the
   source path in the file's header comment.

## Quick orientation (details in data_map.md)

- **Three storage units**: ResearchDrive `adattie` (legacy, near full) and
  `mkeller3` (current projects) on UW ResearchDrive, plus S3 bucket
  `adattie-bucket-01` (2026 production/app data).
- **DO1200** (Diversity Outbred): `RD mkeller3/General/main_directory`,
  documented in repo `sysgenDO1200`.
- **Mark Keller workflows**: `RD mkeller3/General/Projects2`, documented in
  `mkeller3Projects2`.
- **Founder studies** (diet, calcium): `RD adattie/General/founder_*`,
  apps on RStudio Connect, documented in `founder_diet_study`.
- **S3 2026 prod**: miniViewer 3.0 scan matrices (.fst, per chromosome,
  isoforms + metabolites, additive/diet/sex interaction models); dev holds
  DO per-gene top-SNP CSVs and the physiological QTL app backend.
- **Formats**: XLSX→CSV→RDS pipeline historically; qtl2fst (per-chromosome FST
  + RDS controller) for genotypes; sqlite for SNPs/variants; parquet
  migration underway (founder_diet_study DEVELOPER.md has schemas).
- **Analysis tools**: `sysgenAnalysis` (R package: qtl / hotspot / conserve
  workflows).
