# Methods Library

## CSDID Setup (Binary Treatment, Staggered DiD)
1. Define cohort variable (quarter of first treatment, 0 = never treated)
2. Restrict to once-treated units (single shock) — avoids multiple-treatment violations
3. Choose comparison group: notyet (more power) or never-treated (fewer assumptions)
4. Include covariates for conditional parallel trends (demographics, market size)
5. Cluster at the treatment unit level (ZIP, county, etc.)
6. Run `estat event` for event study; capture from `r(table)` for export
7. Test pre-trends: omnibus test + inspect direction of pre-trend coefficients

## IV Diagnostics
- First-stage F-stat: KP rk Wald F. Stock-Yogo 10% threshold: 16.38
- Compare F across subsamples to assess where instrument is strong vs weak
- If F drops dramatically in a subsample, null results there are uninformative

## Pre-Trends Investigation Protocol
1. Run CSDID on first-stage outcomes (e.g., direct treatment measures)
2. Test all 4 variants: notyet +/- covariates x never-treated +/- covariates
3. If omnibus rejects, inspect the **direction**:
   - Pre-trends running opposite to treatment effect --> bias against finding effects (favorable)
   - Pre-trends running same direction as treatment effect --> bias toward false positives (concerning)
4. Compare treated vs control unit characteristics to understand what drives pre-trends
5. Test "clean" subsample (units with no pre-treatment events) to see if effect persists/strengthens
6. Document everything in the project's identification documentation

## Weight Construction for Choice-Based Samples
- When combining a general sample with an oversample selected on an outcome:
  - Weight = P(base) / P(base + oversample) for oversampled group
  - Weight = 1.0 for general population
  - Apply time-varying scale factors if sample composition changes over time
  - Final weight = base_weight x scale_factor

## CSDID vs IV: Different Estimands
- CSDID with binary treatment --> ATT (average treatment effect on the treated)
- IV with continuous treatment --> TWFE-weighted parameter (per-unit marginal effect)
- These need not agree, especially with heterogeneous treatment intensity
- Reference: Callaway, Goodman-Bacon & Sant'Anna (2024)
