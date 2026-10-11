# DOE-Based Screening and Optimization of an SiO₂ Plasma Etch Process

A Design of Experiments (DOE) study that screens six process parameters of a plasma etch recipe and finds settings that maximize **SiO₂ etch rate** and **etch selectivity**, using only **8 runs instead of the 64 a full factorial would need**. Analysis was done in **JMP**.

## Problem

Plasma etch recipes have many interacting knobs, and testing every combination is too expensive. The goal here was to find which parameters matter and set them to get a high etch rate without losing selectivity.

## Experimental Design

- **Design:** 8-run, two-level, six-factor fractional factorial (screening, Resolution III). It is orthogonal and balanced (4 low / 4 high per factor).
- **Factors** (normalized to the maximum value of each):

| Factor | Low | High |
|---|---|---|
| Pressure | 0.5 | 1.0 |
| RF Power | 0.625 | 1.0 |
| CCl₄ Flow | 0.5 | 1.0 |
| He Flow | 0.25 | 1.0 |
| O₂ Flow | 0.5 | 1.0 |
| Electrode Gap | 0.5 | 1.0 |

- **Responses:** Etch Rate (nm/min) and Selectivity (both to be maximized).
- **Data:** [`etch_8runs.csv`](etch_8runs.csv)

**Limitation of the design:** main effects are confounded with two-factor interactions. For example, Pressure is aliased with He × O₂. Screening results therefore point to the important factors, but they do not prove there are no interactions.

## Method

1. Built the data table in JMP and fitted all six main effects (Fit Model, *Effect Screening* emphasis).
2. Screened factors using the Effect Summary (LogWorth), Parameter Estimates and Effect Tests. With 7 parameters on 8 runs the model has only 1 error degree of freedom, so p-values were treated as a ranking rather than strict tests.
3. Dropped RF Power, CCl₄ Flow and Electrode Gap, which together explain about 0.1% of the variation in etch rate, and refitted a 3-factor model (Pressure, He, O₂). This gave 4 error degrees of freedom and reliable p-values.
4. Checked the fit with Actual-by-Predicted and Residual-by-Predicted plots.
5. Optimized both responses together in the Prediction Profiler using desirability functions (maximize both, equal weight).
6. Cross-checked the result against JMP's Easy DOE workflow.

## Results

| Factor | Effect on etch rate (low → high) | Share of variation |
|---|---|---|
| **Pressure** | −224 nm/min | ~64% |
| **O₂ Flow** | −116 nm/min | ~17% |
| **He Flow** | +107 nm/min | ~15% |
| RF Power, CCl₄ Flow, Electrode Gap | negligible | ~0.1% |

- **Etch rate model:** R² ≈ 0.96, adjusted R² ≈ 0.94, RMSE ≈ 38 nm/min. All three retained factors have p < 0.02.
- **Selectivity:** driven mainly by Pressure (lower is better) and O₂ (higher is better). He's effect on selectivity is not significant.
- **Predicted optimum:** Pressure at the low level, He and O₂ at the high level, which gives about **318 nm/min** (95% interval ≈ 243–393) at a selectivity of about **0.78** (≈ 0.60–0.95).
- The three non-significant factors can be fixed at their cheapest tested level.

## Caveats

- The optimum is a corner of the design space that was **not among the 8 runs**, so it is a model prediction. A confirmation run is needed to validate it.
- Results are valid only within the tested ranges.
- A follow-up Response Surface (RSM) design on Pressure, He and O₂ would capture curvature and interactions.

## Repository Contents

```
├── etch_8runs.csv        # 8-run design with measured responses
├── etch_doe.jmp          # JMP data table with saved analysis scripts
├── report/               # exported JMP report (PDF/HTML)
└── README.md
```

## Tools

JMP (Fit Model, Prediction Profiler, Easy DOE)

## Acknowledgement

Dataset and problem statement are from a course assignment (IISc CeNSE).
