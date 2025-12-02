# Vitamin D and Fasting Glucose Analysis in NHANES
This repository contains survey-weighted statistical analyses examining the association between serum vitamin D levels and fasting glucose categories using NHANES data. The analysis includes:

- Survey-weighted ordinal regression (svyolr)

- Survey-weighted binary logistic regression (svyglm, quasibinomial)

Assumption checks for:

- Linearity in the logit (Box–Tidwell approach)

- Multicollinearity (Variance Inflation Factors)

All methods correspond to the Advanced Data Analysis (ADA) final project.
# Project Overview
This project evaluates how serum 25-hydroxyvitamin D (LBXVIDMS) relates to fasting glucose status categorized as:

Normal: < 100 mg/dL

Impaired: 100–125 mg/dL

Diabetic: ≥ 126 mg/dL

Because both vitamin D and glucose are measured in the MEC exam component, all analyses apply the appropriate NHANES complex survey design, including:

Stratification: SDMVSTRA

Primary Sampling Units (Clusters): SDMVPSU

MEC Exam Weights: WTMEC2YR

Using the correct NHANES design ensures that results are nationally representative and valid at the population level.
# Files Included
The repository currently contains the following files:
## 📁 Files Included

The repository currently contains the following files:

```text
.
├── README.md
│   └── Project documentation and overview
├── nhanes_all.csv
│   └── Combined NHANES dataset used for the analysis
└── nhanes_glucose_vitd_assumptions_first.R
    └── Main R script containing:
        - Data preparation
        - Survey design specification
        - Linearity (Box–Tidwell) tests
        - Multicollinearity checks (VIF)
        - Logistic regression models
        - Ordinal regression model

# Repository Structure
Repository Structure
.
├── R/
│   └── nhanes_glucose_vitd_assumptions_first.R
│
├── output/
│   ├── model_results_logistic.txt
│   └── model_results_ordinal.txt
│
├── nhanes_all.csv   (this file must be placed in the project root)
│
└── README.md



# Methods Summary
## A. Data Preparation
The script:

Reads the full analytic dataset (nhanes_all.csv)

Creates a 3-level ordered fasting glucose outcome:
Normal → Impaired → Diabetic
Recodes predictors:

Vitamin D (LBXVIDMS → vitd)

Age (RIDAGEYR → age)

Sex (RIAGENDR → sex)

Race/ethnicity (RIDRETH3 → race)

Performs sanity checks (variable names, summaries)

Missing values are removed only when required for each model.
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

---

## **C. Assumption Checks**
### 1. Linearity in the Logit (Box–Tidwell Method)

Interaction terms evaluated:

vitd * log(vitd)

age * log(age)

Non-significant interaction terms → linearity assumption supported.

### 2. Multicollinearity

Assessed using VIF from the full logistic regression model.
All predictors showed acceptable VIF values (generally VIF < 3).

---

### **D. Statistical Models**
1. Binary Logistic Regression

Two survey-weighted models:

Model 1:
Normal (0) vs Impaired/Diabetic (1)

Model 2:
Diabetic (1) vs Normal/Impaired (0)

Models fitted using:
svyglm(..., family = quasibinomial())
Odds ratios and 95% confidence intervals are calculated using exponentiated coefficients.

A coefficient comparison table evaluates whether associations are similar across outcome thresholds.

2. Survey-Weighted Ordinal Logistic Regression
A proportional-odds model is fitted using:
svyolr(glu_cat ~ vitd + age + sex + race, design = nhanes_svy)
The ordinal model provides a single set of odds ratios representing the cumulative odds of being in higher fasting glucose categories.

### E. OUTPUT FILES
All results are automatically written to the output/ directory.

model_results_logistic.txt
Contains summaries, odds ratios, confidence intervals, and VIF results for Models 1 and 2.

model_results_ordinal.txt
Contains the proportional-odds model output and exponentiated coefficients.
# Running the Analysis
1. Install Required Packages
   install.packages(c("tidyverse", "survey", "car"))
2. Run the Full Analysis Script
   source("R/nhanes_glucose_vitd_assumptions_first.R")
All model outputs will appear in the output/ folder.

# **NHANES Reference**
National Health and Nutrition Examination Survey (NHANES)
Centers for Disease Control and Prevention (CDC)
https://www.cdc.gov/nchs/nhanes.htm

---

# **Author**
Abrham Azale, MD, MPH(c)
Washington University in St. Louis
2025




