# Project: [Your Project Name]

## Before Any Code Changes
1. Read `docs/knowledge/INDEX.md` — single source of truth for all project knowledge
2. Read `code/config.do` — project directory globals used by all do-files
3. For cross-project patterns: see `~/claude-knowledge/`

## Do-File Conventions
- Every do-file sources `code/config.do` at the top for path globals
- Use existing working do-files as templates — match their structure, variable names, and style exactly
- Do NOT reorganize, refactor, or add complexity beyond what was requested

## Key Variables
- `treatment_var`: [describe your treatment variable]
- `instrument_var`: [describe your instrument, if applicable]
- `cohort_var`: [describe cohort variable for CSDID, if applicable]

## Directory Structure
- `code/` — Stata do-files (numbered steps, estimation scripts)
- `data/raw/`, `data/intermediate/`, `data/analysis/` — data pipeline stages
- `output/tables/`, `output/figures/`, `output/logs/` — results
- `docs/knowledge/` — single source of truth (topic-based files)
- `docs/` — output products (LaTeX, PDFs)

## Knowledge Management
- Every fact lives in exactly one place: `docs/knowledge/`
- Paper notes in `docs/knowledge/papers/` are pure summaries — project implications go in the relevant topic file
- Output products (LaTeX, emails) are derived from the knowledge repo, not duplicated
