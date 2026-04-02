---
name: scope
description: Set the autonomy scope for the current session. Controls how much Claude does before stopping to check in with the user. Invoke as /scope full, /scope moderate, or /scope limited.
user_invocable: true
---

# Scope Control

Set the autonomy scope for the current session. This controls how much work Claude does before stopping to report and ask for direction.

## Arguments
- `full` — No scope restrictions. Default Claude Code behavior. For purely mechanical tasks where judgment is not needed (moving files, running a pre-agreed script, formatting).
- `moderate` — Complete one coherent unit of work, then stop and report. Ask what to do next. A "unit" is one file edit, one script run, or one analysis question answered. Use judgment about when to stop, with a strong prior toward stopping sooner rather than later. Do not chain multiple units of work.
- `limited` — Atomic tasks only. Do exactly one non-read action, report it, stop. No judgment about what comes next — the user decides everything. Appropriate when analytical decisions are being made, when user and Claude are out of sync, or at the start of an unfamiliar task.

## Default
If `/scope` has not been invoked in a session, the default level is **limited**.

## Behavior

When this skill is invoked:

1. Acknowledge the scope level.
2. For the remainder of the session (or until `/scope` is invoked again), operate at the specified level.
3. Note the active scope level at the top of every response so the user can see it and adjust if needed.

### At each level:

**full:**
- Execute multi-step tasks as a sequence.
- Report results when done or when stuck.
- Still obey all feedback memories and the pre-action feedback review rule.

**moderate:**
- After completing one coherent unit of work, stop.
- Report what was done in 1-3 sentences.
- Ask: "What would you like me to do next?"
- Do not anticipate the next step. Do not start the next unit.

**limited:**
- Perform exactly one non-read action (one edit, one bash command, one file write).
- Report what was done.
- Stop. Wait for the user.
- If the user asks a question, answer it. Do not follow up with action.
- If analysis or investigation seems needed, say so. Do not begin it.

## Important
- The scope level does NOT override the pre-action feedback review rule. All feedback memories must still be checked before any non-read action at every scope level.
- The scope level does NOT override the requirement to verify claims before stating them as fact.
- Scope controls *quantity* of work per turn, not *quality*. Quality standards are absolute regardless of scope.
- The user can change the level at any time by invoking `/scope` again.

## Why This Exists

Claude Code operates at machine speed, which eliminates the space where the researcher's judgment should operate between steps. In empirical research, the thinking between steps is where the real work happens — evaluating results, considering identification, deciding what to run next. If Claude skips that space, it substitutes its momentum for the researcher's judgment, creating more work rather than saving it.

The `/scope` skill gives the researcher a dial to control this. Most analytical work should run at `limited` or `moderate`. Only purely mechanical execution warrants `full`.
