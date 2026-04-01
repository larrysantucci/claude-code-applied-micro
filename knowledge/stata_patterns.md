# Stata Technical Patterns

## csdid (Callaway & Sant'Anna 2021)
- **Memory limit**: crashes on >2.5M person-quarter obs even with 198GB RAM / 48 vCPUs
- Stata-MP license caps at 8 cores; csdid uses ~2.5 cores
- **saverif() cannot have directory paths** *(verified)*: `saverif(path/to/rif_file)` crashes with `/ invalid name` (rc=198). Use bare filenames only: `saverif(rif_file)`. The `/` characters in the path cause csdid to create invalid internal variable names. Move files after the run if needed.
- `estat event` restores original csdid e() results — must capture from `r(table)` matrix, not `_b[]`/`_se[]`
- `estore()` option crashes on unbalanced panels — use manual `r(table)` capture
- `saverif()` filename must be <=32 chars total (Stata name limit); use short aliases
- **`estat simple` overwrites `estat event` results** *(verified)*: `csdid_plot` reads from the stored `estat event` results. Running `estat simple` (or `estat group`) between `estat event` and `csdid_plot` overwrites those results, causing `csdid_plot` to emit `} is not a valid command name` and fail to create a new graph. `graph export` then exports whatever graph was previously in memory (stale content from prior outcome). Correct order: `estat event` -> `csdid_plot` -> `graph export` -> `estat simple`.
- Wild bootstrap with `csdid_stats` requires loading saved RIF file; use `preserve/restore` pattern
- Uses base-period covariate values: G-1 for post-treatment ATTs, t-1 for pre-treatment ATTs
- Supports sampling weights via `[iw=weight]` — internally treated as pweights per help file

## ivreghdfe
- Uses ~7 cores; handles large pooled samples (5M+ obs) without issues
- Standard syntax: `ivreghdfe outcome (treatment=instrument) [pweight=weight], absorb(geography state#time individual) cluster(geography)`

## General Stata
- `putexcel` can crash on large outputs — save coefficients from log as backup
- For person-level random numbers in panel data: generate for `_n==1` per person only, then `bysort id: replace rand = rand[1]`. Do NOT use `bysort id (rand): replace rand = rand[1]` — this takes the MIN of all draws.
