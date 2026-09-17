# Heureka Resources Overview

**Prompt:**
Tell me about resources in Heureka.

## Purpose

This document orients a data scientist (working with Alan Attie) to the resources and capabilities available in Heureka Bench and its ARC agent. It is organized by functional category rather than by tool name, since the underlying toolset evolves.

## 1. Project Structure and Context

Heureka Bench organizes work into **projects** — folders on disk under a workspace root, each with a `.heureka/` directory holding:

- [**RATIONALE.md**](RATIONALE.md) — goal, background, approach, and success criteria for the project, plus an auto-generated list of linked papers.
- **MEMORY.md** — persistent research memory: findings, decisions, issues, and next steps that carry across sessions.
- **todos/** — a durable task list visible in the app's To-Dos view.
- **plans/** — pre-registered, locked methodology files created when an analysis is planned before execution.
- **experiments.jsonl** — a running log of analyses performed, with parameters, inputs, and outputs, for reproducibility.
- **figures/** — auto-saved plot outputs.

A separate **Paper Library** exists outside project structure for organizing PDFs, each with a companion notes/summary/methods/citation folder.

## 2. Data and Dataset Handling

- **Data profiling** — instant statistical summaries of any dataset (CSV, TSV, Parquet, Excel, HDF5, FASTA, VCF, etc.): shape, types, missingness, correlations, quality flags. Meant to run before any analysis touches a file.
- **Dataset fetching** — direct pulls from public bio-repositories (GEO, SRA, ENA, PDB, UniProt, Ensembl, ENCODE, GTEx) into the project's `data/raw/` with checksums and automatic provenance logging.
- **Database queries** — structured lookups against UniProt, PubChem, Ensembl, ClinVar, KEGG, and AlphaFold for protein, gene, compound, pathway, and structure information.
- **Sequence analysis** — DNA/RNA/protein operations: translation, reverse complement, GC content, molecular weight, ORF finding.

## 3. Statistical and Analytical Rigor

Heureka is built around pre-registration and audit discipline, not just running code:

- **Plan mode** — commits to a methodology (test, primary outcome, confounders, stopping rule) before results are seen; the plan is hashed and locked as an immutable record.
- **Confounder declaration** — formal registry of confounders, mediators, moderators, and colliders tied to a plan, feeding into limitations sections and bias scoring.
- **Sample size / power calculation** — required N, achieved power, or minimum detectable effect for standard tests (t-tests, ANOVA, chi-square, correlation, logistic regression, Cox).
- **Statistics calculator** — test recommendation, effect sizes (Cohen's d, Hedges' g, Cliff's delta, odds ratios), multiple-comparison correction (Bonferroni, Holm, BH/BY-FDR), family-wise error rate, and calibrated result interpretation.
- **Drift detection** — compares logged analyses against the pre-registered plan and flags deviations (HARKing, post-hoc subgrouping, dropped covariates).
- **Causal DAG tool** — Pearl-style adjustment-set computation (backdoor/front-door criteria) for causal claims.
- **Criteria checker** — declares and audits inclusion/exclusion criteria against a dataset, with pass/fail on the pre-registered sample size.
- **Risk-of-bias scoring** — Cochrane RoB 2, ROBINS-I, SYRCLE, JBI frameworks for evaluating study quality.
- **Reporting checklists** — CONSORT, PRISMA, STROBE, ARRIVE, MIQE audits before manuscript drafting.

## 4. Literature and Citation Management

- **Literature search** — across PubMed, arXiv, and Semantic Scholar with structured metadata (abstracts, DOIs, citation counts).
- **Citation formatting** — DOI/PMID/arXiv lookups rendered in BibTeX, APA, Vancouver, Nature, and other styles.
- **Citation validation** — checks DOIs/PMIDs against Crossref and NCBI to catch fabricated or mismatched references before a draft goes out; maintains a canonical project `.bib` file.

## 5. Code Execution and Analysis Environments

- **Persistent REPL/notebook execution** — Python, R, Julia, Node, with state retained across calls; automatic capture of plots (matplotlib, ggplot).
- **Jupyter notebook editing** — cell-level insert/replace/delete operations on `.ipynb` files.
- **Shell access** — for pipelines, package management, and anything genuinely process-shaped (git, build tools, bioinformatics CLIs).
- **Pipeline runner** — executes project-registered analysis pipelines (e.g., RNA-seq differential expression, scRNA-seq QC) with parameter validation and auto-logging.
- **Cloud jobs** — offloads heavy or long-running analyses to more capable remote compute when local resources are insufficient.

## 6. Multi-Omics Grounding: the Archimedes Model

A proprietary multi-omics model queried as a graph of molecular biology — genes, CpG sites, microRNAs, and samples linked to each other and to phenotypes. Capabilities include:

- Sample stratification and nearest-neighbor lookup against reference tissue/disease states.
- Annotation mismatch / novelty detection for a sample's claimed label.
- Metadata prediction (tissue, disease, relative survival risk) from an expression profile.
- Marker gene retrieval for a tissue or disease.
- Survival-associated gene ranking.
- Gene neighbor (co-expression), co-essentiality (CRISPR fitness), and mutation-dependency traversal.
- CpG and microRNA annotation, co-methylation/co-expression neighbors, and candidate target genes.
- Cross-omic hops (e.g., CpG to microRNA via shared gene).

All outputs are associational, confidence-labeled, and meant as leads to corroborate — not causal claims. This is best used for grounding specific molecular questions (a gene, a CpG, a sample profile), not as a general-purpose lookup.

## 7. Manuscript and Communication Output

- **Manuscript section generation** — Methods (from experiment logs), Results (from pipeline outputs), Introduction (from rationale + literature), Discussion (from hypothesis status) — assembled from actual project artifacts, not free-form generation.
- **Presentations** — markdown-based slide decks stored per-project, rendered in the app's Presentations view.
- **Notes vault** — atomic, wiki-linked knowledge notes distinct from project memory, useful for capturing single findings or concepts with cross-links.

## 8. Knowledge and Provenance Tracking

- **Experiment log** — records what was run, with what parameters, on what data, producing what outputs; queryable by tag, file, or date.
- **Research memory (knowledge graph)** — entities (genes, compounds, hypotheses, experiments, findings, papers) and relationships between them, queryable by connection, timeline, or hypothesis status.
- **Provenance tracking** — lineage graph linking derived files/figures back to their source data, samples, or experiments.

## 9. Lab Operations

- **Inventory management** — structured chemical/reagent inventories with automatic structure rendering (SMILES/InChI columns) and hazard lookups (GHS classification).
- **Animal colony management** — cages, matings, genotypes, census tracking.
- **Data Spaces** — promoting a mature, complete project into a shareable, citable dataset card with cross-dataset concept linking.

## 10. Chemistry-Specific Tools

- **Compound resolution** — canonical structure, formula, mass, and identifiers (InChIKey, CAS) from any name, SMILES, or identifier — the recommended way to avoid hand-written (and potentially wrong) structures in any document.
- **Safety data** — GHS hazard classification, always presented as a pointer to the SDS/institutional policy, not a safety clearance.
- **Bioactivity lookup** — curated ChEMBL targets, mechanism of action, and clinical phase.
- **Similarity search** — structural analog discovery by Tanimoto similarity, for hypothesis generation (not activity prediction).

## Practical Takeaway for This Project

Given the stated goal of learning Heureka's resources and capabilities, a useful next step is to pick one small piece of actual project data (even a toy dataset) and walk it through the pipeline end-to-end: profile → plan → analyze → log → cite → draft. That exercise surfaces how the pieces interlock better than any static list.
