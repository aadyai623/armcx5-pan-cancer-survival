# ARMCX5 Pan-Cancer Survival Analysis

Does ARMCX5 expression predict patient survival outside of breast cancer, and does that answer change once you control for age and tumor stage?

## Background

A recent study found that ARMCX5 expression was associated with high-grade, more aggressive breast tumors in TCGA data, but flagged the gene as unexplored beyond that cohort. This project tests whether that prognostic signal generalizes to other cancer types, and whether a simple univariate test tells the same story as a model that accounts for known confounders.

## Method

**Data source:** UCSC Xena, using TCGA RNA-seq (STAR, FPKM-UQ) expression data, matched clinical survival data (overall survival / OS.time), and clinical covariates (age at diagnosis, tumor stage).

**Cancer types tested:** LUAD (lung), OV (ovarian), KIRC (kidney clear cell), SKCM (melanoma), UCEC (endometrial).

**Analysis, two stages:**

1. Univariate: patients split into high vs low ARMCX5 expression at the median. Survival compared using the Kaplan-Meier estimator and log-rank test.
2. Multivariate: a Cox proportional hazards model tested ARMCX5 expression (continuous), patient age, and tumor stage (AJCC or FIGO, simplified to stages I to IV) simultaneously, isolating ARMCX5's independent effect on survival.

**Tools:** Python (pandas, lifelines, matplotlib).

## Results

| Cancer Type | N | Log-rank p-value | Cox-adjusted p-value | Significant (Cox, p<0.05) |
|---|---|---|---|---|
| LUAD | 557 | 0.2832 | 0.9690 | No |
| OV | 307 | 0.2232 | 0.0798 | No, trending |
| KIRC | 602 | 0.9071 | 0.4070 | No |
| SKCM | 407 | 0.1768 | 0.0361 | Yes |
| UCEC | 521 | 0.4356 | 0.1456 | No |

In every cancer type tested, age and tumor stage were themselves strongly significant predictors of survival (p<0.005 in all five), confirming the models had real statistical power to detect known clinical effects. The null results for ARMCX5 in four of five cancers are not simply a case of underpowered models.

## Key finding

The univariate log-rank test alone would suggest ARMCX5 has no prognostic value outside breast cancer. But in melanoma (SKCM), the Cox model, which adjusts for age and stage, revealed a statistically significant, independent association between ARMCX5 expression and survival (p = 0.036) that the simple test missed. This suggests confounding by age and/or stage was masking a real effect in the univariate analysis, and that ARMCX5's prognostic relevance may extend beyond breast cancer into at least one additional tissue type.

## Interpretation

ARMCX5's prognostic value appears to be neither purely breast-specific nor a universal cancer biomarker. It is tissue and model dependent. The melanoma finding raises an open question for follow-up: what melanoma-specific biology, such as immune microenvironment or melanocyte-specific signaling, might explain an independent ARMCX5 effect there. The OV result (p = 0.08, trending) is also worth flagging as a candidate for a larger sample or independent validation cohort rather than a confirmed null.

## Repository Contents

- `cancer.ipynb`: full analysis notebook (data loading, log-rank tests, Cox models, plots, for all five cancer types)
- `armcx5_results.csv`: summary results table

## Limitations

- Single database analysis (TCGA only), no external validation cohort
- Tumor stage was simplified from detailed AJCC/FIGO substages (e.g. Stage IIIA) to a single ordinal variable (I to IV); finer grained staging or additional covariates (sex, treatment history) were not modeled
- Median split expression grouping in the univariate test is a simplification; results may differ under other cutoffs (tertiles, quartiles)
- The SKCM finding is a single significant result among five tests; it has not been corrected for multiple comparisons and should be treated as hypothesis generating rather than confirmatory
