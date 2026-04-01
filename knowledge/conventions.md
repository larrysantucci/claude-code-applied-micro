# Process Conventions

## Verify Every Consequential Step
After any operation that should produce a specific, predictable result — filtering, sampling, merging, variable creation, aggregation — immediately add a check that confirms the result matches expectations. Display counts, percentages, summary statistics, or distributions. If the output doesn't match what the logic implies, stop and debug before proceeding.

Examples:
- After subsampling: display before/after count and actual retention rate
- After a merge: display matched/unmatched and verify they're reasonable
- After creating a variable: display summary stats (mean, min, max, distribution)
- After filtering: display how many observations were dropped vs expected
- After any group-level operation: verify the result on a few cases

## Review Estimation Packages Before Running
Before using any estimation package in any language (Stata, R, Python), fully review the documentation together. Come to an explicit agreement on:
1. Which estimation options to use (comparison groups, clustering, controls, etc.)
2. Which post-estimation commands to run (and which to skip)
3. What output to report and where each result appears in the document

Do this BEFORE writing any code. This prevents discovering unreported options late.

## Code Conventions
- Do NOT reorganize, refactor, or add complexity beyond what was requested
- Use existing working files as templates — match structure, variable names, and style exactly
- Show diffs before running anything destructive
- Prefer editing existing files over creating new ones

## Knowledge Management
- Every fact lives in exactly one place (the project knowledge repo)
- Paper notes are pure summaries — project implications go in the relevant topic file
- Output products (LaTeX, emails) are derived from the knowledge repo, not duplicated
- When knowledge is discovered or corrected, update the canonical source immediately

## Communication Preferences
- Provide context before asking detailed implementation questions
- Minimal text in LaTeX — tables and figures tell the story
- When referencing files, use markdown links with line numbers

## Skill and Hook Opportunities
When any of these conditions are met, propose creating a new skill or hook:
- A workflow has been performed 3+ times with the same general structure
- A manual check is being repeated after every run
- A communication product (email, memo, LaTeX section) follows a consistent template
- An error pattern has been caught and fixed more than once (candidate for a validation hook)
- A "read these files first" preamble is being repeated across tasks (candidate for a skill that automates context-loading)

When proposing, specify: skill vs hook, trigger condition, what it automates, and which layer it belongs to (global `~/.claude/` vs project `.claude/`).

## PDF Output
- Always date-stamp compiled PDFs: `filename_DDMMMYYYY.pdf`
- Keep the non-stamped filename as the working copy; the stamped copy is the snapshot
- After every compile, visually inspect the PDF before presenting it. Check for: table alignment, note width consistency, content overflow/clipping, page break placement, spacing issues, and formatting consistency across all pages.

## Subsampling Rules
- Person-level subsampling only (never observation-level) for panel data
- One random draw per person, copied to all periods
- Always verify retention rate after subsampling
