# Lessons Learned

Hard-won debugging insights from real projects. Each entry documents a specific bug, its root cause, the fix, and a generalizable lesson.

## Subsampling Bug
**Problem**: `bysort ind_id (rand): replace rand = rand[1]` takes the MINIMUM of ~30 uniform draws per person (expected value ~0.03), so `keep if rand <= 0.20` retained ~99% instead of 20%.
**Fix**: Generate rand for `_n==1` per person only, then fill down.
**Lesson**: Always verify retention rate after subsampling. The "Verify Every Consequential Step" convention catches this.

## Inherited Sample Restrictions
**Problem**: CSDID and IV regressions were run on an oversample subset only — this was NOT requested by the research team. It was copied from an early test script by a previous Claude session.
**Lesson**: When adapting code from an existing script, verify ALL sample restrictions against the research design, not just the new changes. Inherited assumptions are invisible.

## Memory Crashes in Large Panel Estimations
**Problem**: Person-level `csdid` crashes on large pooled samples (>2.5M obs) even with 198GB RAM.
**Solution**: Separate subsamples. Run smaller subsamples at varying rates.
**Lesson**: Test computational feasibility on a small subsample before committing to a full run. Document memory limits in the methods library.

## putexcel Crash Recovery
**Problem**: Panel estimation completed all regressions but Stata crashed during putexcel export.
**Solution**: Recovered coefficients from log file.
**Lesson**: Always run with `log using`, and consider saving coefficients to a .dta file *during* the loop, not just at the end via putexcel.

## Pre-Trends Direction Matters
**Problem**: Omnibus pre-trend test rejects — does this invalidate results?
**Resolution**: No — the direction of pre-trends matters. When pre-trends run opposite to the treatment effect, they bias toward null. Document the direction argument with data (treated vs control characteristics table).
**Lesson**: Pre-trend rejection is not a binary pass/fail. Always inspect the direction and magnitude.

## CORRECTED: Stata Locals Are NOT Wiped by `use ..., clear`
**Original claim**: `tempfile` and `local` macros are destroyed when `use ..., clear` loads new data.
**Correction**: This was a misdiagnosis. Empirical testing on Stata 18 MP confirms that `use ..., clear` does NOT destroy local macros or tempfile handles. Even `clear all` does not wipe locals within the same do-file scope. The original bug that prompted a do-file rewrite (converting locals to globals) must have had a different root cause — likely a typo, scoping issue, or unrelated error.
**Test** (run this yourself to verify):
```stata
clear all
set more off
local x "hello"
tempfile tmp
save `tmp', emptyok
sysuse auto, clear
display "local x after use..clear: [`x']"       // -> "hello" (SURVIVES)
display "tempfile after use..clear: [`tmp']"     // -> path (SURVIVES)
```
**Lesson**: Never record a causal explanation without a controlled test. When a "fix" works, verify that the diagnosed cause was actually the problem — coincidental fixes encode false beliefs that propagate to future sessions.

## Review Estimation Package Options Before Running
**Problem**: Ran `csdid` and reported Post_avg and Pre_avg from `estat event` without systematically reviewing all post-estimation options. The overall ATT (`estat simple`) was captured in code but never introduced or explained. Group-level and calendar-time ATTs were never run at all.
**Lesson**: Before using ANY estimation package in any language (Stata, R, Python), fully review the documentation together. Come to an explicit agreement on: (1) which options to run, (2) what output to report, and (3) where each result will appear. This applies to estimation AND post-estimation commands. Do this BEFORE writing any code.

## Information Scatter
**Problem**: Same information documented in 3+ places (README, research summary, paper notes, memory). Led to attempting to document something that already existed.
**Solution**: Three-layer knowledge architecture (Claude instructions / Knowledge repo / Output products). Every fact lives in one place.
**Lesson**: Establish knowledge management structure at project start, not after information has accumulated.

## Hardcoded Absolute Paths Break on Migration
**Problem**: Project moved to a new directory. Config file had a hardcoded absolute path for the working directory, plus references scattered in documentation.
**Fix**: Updated all occurrences. Could have been avoided with relative paths or environment variables.
**Lesson**: Use relative paths or `here::here()` (R) / environment variables where possible. When absolute paths are unavoidable, centralize them in one config file.

## Retroactive Project Organization Is Expensive
**Problem**: After 10 months of development, dozens of scripts accumulated at the project root with no subdirectory structure. Markdown files spread across multiple locations. Reorganizing required moving many files and updating cross-references.
**Lesson**: Establish `scripts/`, `docs/knowledge/`, and a project-level CLAUDE.md at project start. The Information Scatter lesson (above) applies to code files too, not just documentation.

## Research Data Availability Before Coding
**Problem**: When adding a new Census variable to a data pipeline, code was modified and run before researching availability. The API returned errors for pre-2020 vintages. Instead of stopping to investigate, the code was patched to silently set 6 years to missing.
**Lesson**: When adding any new external data source or variable, fully research its availability across all relevant dimensions (years, geographies, table codes, API endpoints) BEFORE writing code. If there are gaps or limitations, present findings to the user with options. Never silently set data to missing — that's a consequential analytical decision that belongs to the researcher, not the assistant.

## Never Reverse a Technical Position Without New Evidence
**Problem**: Claude correctly assessed a Stata behavior, then reversed that assessment when the user pushed back — citing a lessons.md entry that was itself a misdiagnosis from a prior session. A 30-second test confirmed the original position was right.
**Root cause**: Social pressure (user pushback) was treated as evidence. A prior session's documented "lesson" was treated as ground truth without verification.
**Lesson**: When a technical claim is challenged, do not flip — investigate. Prior Claude session notes are hypotheses, not facts. Run a controlled test when possible. Only reverse after dispositive evidence, never after pressure alone.

## Verify Subagent Claims Before Reporting
**Problem**: A coder-critic subagent reported Critical issues based on a false premise about Stata behavior. These were presented to the user without independent verification.
**Lesson**: For every Critical or Important finding from a subagent, verify the claimed *mechanism* — not just that the code looks as described, but that the consequence actually follows. Label confidence explicitly: "Confirmed by code reading," "Plausible but untested," "Needs runtime test."

## Use Toy Code to Resolve Uncertain Mechanisms
**Problem**: Code audit findings had uncertain provenance — documented behaviors may have originated from the same session that produced a disproven claim. Code reading alone couldn't resolve whether predicted consequences were real.
**Solution**: Write minimal self-contained scripts (~15 lines) that isolate the exact mechanism and test it on toy data.
**Lesson**: When documentation and code reading leave a mechanism uncertain, write a toy test. 15 lines of dispositive evidence beats hours of reasoning from documentation. The cost of a controlled test is almost always lower than the cost of acting on an unverified assumption.

## Pre-Filter Diagnostics Reveal True Data State
**Problem**: Sequential filter reporting said "62,442 nominals removed, then 25,044 exclusion flags, then 2,118 zero consideration." This hid the fact that a much larger number of records had zero consideration — most were also nominal and got counted there first.
**Fix**: Before applying any sequential filters, run independent prevalence counts for each condition and a pairwise overlap matrix. Document both pre-filter diagnostics AND sequential attrition.
**Lesson**: When applying multiple filters to a dataset, always compute each filter condition's prevalence independently BEFORE filtering. Sequential attrition tables describe code execution order; pre-filter diagnostics describe data quality. You need both.
