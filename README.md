# Vitamin D and Fasting Glucose Analysis in NHANES(2011-2018)
This repository contains a fully reproducible statistical analysis examining the association between serum vitamin D levels and fasting glucose categories among U.S. adults using NHANES data from 2011–2018.
The primary model is a survey-weighted ordinal logistic regression (svyolr), with appropriate assumption checks and diagnostics.

This project was developed as part of the Advanced Data Analysis (ADA) final project at Washington University in St. Louis.All methods correspond to the Advanced Data Analysis (ADA) final project.

# Project Overview
This project evaluates how serum 25-hydroxyvitamin D (LBXVIDMS) relates to fasting glucose categories:

- Normal: < 100 mg/dL

- Impaired: 100–125 mg/dL

- Diabetic: ≥ 126 mg/dL

Because both vitamin D and fasting glucose are measured in the NHANES MEC exam component, the analysis incorporates the correct complex survey design:

- Stratification: SDMVSTRA

- Primary Sampling Units (Clusters): SDMVPSU

- Sample Weights: WTMEC2YR

Using the NHANES design ensures nationally representative and unbiased estimates.


##  Files Included

The repository currently contains the following files:

```text
.
├── README.md
│   └── Project documentation and overview
├── nhanes_all.csv
│   └── Combined NHANES dataset used for the analysis
└── nhanes_glucose_vitd-R script.txt
    └── Main R script containing:
        - Data preparation
        - Survey design specification
        - Proportional-odds assumption check (Brant test)
        - Multicollinearity checks (VIF)
        - Survey-weighted ordinal logistic regression
        - Export of all results to the output/ directory
```
# Methods Summary
## A. Data Preparation

The script:

- Loads the analytic dataset (nhanes_all.csv)

- Creates an ordered 3-level fasting glucose outcome:
      Normal → Impaired → Diabetic

-Recodes predictors:

  - vitd (vitamin D in nmol/L)

  - age (years)

  - sex (Male/Female)

  - race (NHANES race/ethnicity categories)

- Performs basic sanity checks

- Handles missingness only when necessary (e.g., complete-case dataset for diagnostics)
## B. Survey Design
NHANES survey design was defined as:

svydesign(
  id      = ~SDMVPSU,
  strata  = ~SDMVSTRA,
  weights = ~WTMEC2YR,
  nest    = TRUE,
  data    = df
)

This provides unbiased variance estimation using Taylor linearization.


## **C. Assumption Checks**
### Three key assumptions were evaluated using unweighted diagnostic models (because survey-weighted versions of these tests are not available):

4a. Proportional-Odds Assumption (Brant Test)

An unweighted ordinal logistic regression using polr() is fitted, followed by a Brant test:

- Non-significant results indicate that the parallel slopes assumption is reasonable.

- Significant results are discussed as a limitation, since the Brant test cannot incorporate survey weights.

4b. Multicollinearity (Variance Inflation Factors)

Using an unweighted linear model with the same predictors:
lm(as.numeric(glu_cat) ~ vitd + age + sex + race)
VIFs are computed using car::vif().

  - VIF < 3–5 indicates no problematic multicollinearity.
    
4c. Influential Observations

Influential cases are identified using:

- Cook’s distance

- Leverage (hat values)

Cutoffs used:

- Cook’s Distance > 4/n

- Hat values > 2p/n

A summary of potentially influential observations is provided.


### **D. Statistical Models**
Survey-Weighted Ordinal Logistic Regression

The main model is:
svyolr(glu_cat ~ vitd + age + sex + race, design = nhanes_svy)
This proportional-odds model estimates the effect of vitamin D (and covariates) on the cumulative odds of being in a higher fasting glucose category.

Results include:

- Regression coefficients

- Exponentiated odds ratios (ORs)

- 95% confidence intervals

### E. OUTPUT FILES
All results are automatically saved to the output/ directory.

model_results_ordinal.txt includes:

- Survey-weighted ordinal regression summary

- Odds ratios and confidence intervals

-  Brant test results

-  Multicollinearity (VIF)

 - Influential observation diagnostics

# Running the Analysis
1. Install Required Packages
   install.packages(c("tidyverse", "survey", "MASS", "brant", "car"))
2. Run the Full Analysis Script
   source("nhanes_glucose_vitd.R")
Results will appear in the output/ folder.

# **Reference**
National Health and Nutrition Examination Survey (NHANES)
Centers for Disease Control and Prevention (CDC)
https://www.cdc.gov/nchs/nhanes.htm

---

# **Author**
Abrham Azale, MD, MPH(c)
Washington University in St. Louis
2025




