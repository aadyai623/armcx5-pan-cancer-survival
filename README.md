# ARMCX5 Pan-Cancer Survival Analysis

Does ARMCX5 expression predict patient survival outside of breast cancer?

## Background

A recent study found that ARMCX5 expression was associated with high-grade,
more aggressive breast tumors in TCGA data, but flagged the gene as
unexplored beyond that cohort. This project tests whether that prognostic
signal generalizes to other cancer types, or whether it's specific to
breast tissue.

## Method

- **Data source:** UCSC Xena, using TCGA RNA-seq (STAR, FPKM-UQ) expression
  data and matched clinical survival data (overall survival / OS.time)
- **Cancer types tested:** LUAD (lung), OV (ovarian), KIRC (kidney clear
  cell), SKCM (melanoma), UCEC (endometrial)
- **Analysis:** For each cancer type, patients were split into high vs.
  low ARMCX5 expression groups at the median. Survival between groups was
  compared using the Kaplan-Meier estimator and log-rank test.
- **Tools:** Python (pandas, lifelines, matplotlib)

## Results

| Cancer Type | N (high) | N (low) | p-value | Significant (p<0.05) |
|---|---|---|---|---|
| LUAD | 290 | 286 | 0.2832 | No |
| OV | 154 | 153 | 0.2232 | No |
| KIRC | 302 | 303 | 0.9071 | No |
| SKCM | 228 | 231 | 0.1768 | No |
| UCEC | 279 | 290 | 0.4356 | No |

ARMCX5 expression did not significantly predict survival in any of the
five cancer types tested, including UCEC (endometrial), a hormone-driven
cancer chosen specifically to test whether ARMCX5's effect might relate
to hormone signaling.

## Interpretation

Combined with the original breast cancer finding, these results suggest
ARMCX5's prognostic value is likely tissue-specific rather than a general
cancer biomarker. This raises an open question about what breast-specific
biology (tissue microenvironment, hormone receptor signaling, or
something else) might explain the discrepancy — a direction for future
investigation, potentially using breast cancer molecular subtype data or
gene co-expression analysis.

## Repository Contents

- `armcx5-pan-cancer-survival.ipynb` — full analysis notebook
- `armcx5_results.csv` — summary results table

## Limitations

- Single-database analysis (TCGA only); no external validation cohort
- Survival comparisons are unadjusted for age, tumor stage, or other
  clinical covariates
- Median-split expression grouping is a simplification; results may
  differ under other cutoffs (tertiles, quartiles)
