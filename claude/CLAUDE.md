# Global Instructions

## Pre-Action Feedback Review (MANDATORY)
Before ANY non-read action — writing code, presenting findings, finalizing a plan, running a script, making a technical claim — read every entry in the Feedback section of MEMORY.md and verify none are violated by the proposed action. For each entry that applies, note how the action complies. This check must be visible in the conversation (in the plan file for plans, or as a brief inline note for other actions). There are no exceptions and no actions too minor to check. This rule exists because feedback memories that are stored but not consulted at the moment of action are worthless.

**Cost justification:** The compliance check costs ~200 tokens. A single error it could have prevented typically costs 5,000–15,000 tokens in rework (user pushback, re-reading files, re-analysis, re-presentation). The rule pays for itself roughly 25–75:1 on any action where it catches an error. Even when it catches nothing, 200 tokens per action is trivial overhead against session totals of 100,000+. Prevention is always cheaper than correction.

## Cross-Project Knowledge
See `~/claude-knowledge/` for conventions, analytical patterns, technical notes, and lessons learned. This repo is shared across all projects.

## Verify Every Consequential Step
After any operation that should produce a specific, predictable result — filtering, sampling, merging, variable creation, aggregation — immediately add a check that confirms the result matches expectations. Display counts, percentages, summary statistics, or distributions. If the output doesn't match what the logic implies, stop and debug before proceeding.

## Report Outputs
- All reports written to files (code audits, peer reviews, insights, etc.) MUST include a datetime stamp in the filename. Format: `<name>_YYYY-MM-DD_HHMM.<ext>`. Never overwrite a previous report — always create a new timestamped file.
- After `/insights` runs: immediately copy `~/.claude/usage-data/report.html` to `~/.claude/usage-data/report_YYYY-MM-DD_HHMM.html` (using current date/time). The built-in command overwrites `report.html`; the copy preserves the historical version.

## Knowledge Management
- Every fact lives in exactly one place (the project's `docs/knowledge/` directory)
- Paper notes are pure summaries — project implications go in the relevant topic file
- Output products (LaTeX, emails) are derived from the knowledge repo, not duplicated
- When knowledge is discovered or corrected, update the canonical source immediately
- Cross-project lessons go in `~/claude-knowledge/lessons.md`
