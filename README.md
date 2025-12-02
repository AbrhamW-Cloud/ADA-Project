# Vitamin D and Fasting Glucose Analysis in NHANES
This repository contains survey-weighted statistical analyses examining the association between serum vitamin D levels and fasting glucose categories using NHANES data. The analysis includes:

-Survey-weighted ordinal regression (svyolr)
-Survey-weighted logistic regression (svyglm, quasibinomial)
Assumption checks for:
-Linearity in the logit (Box–Tidwell approach)
-Multicollinearity (Variance Inflation Factors)
# Project Overview
This project evaluates how serum 25-hydroxyvitamin D (LBXVIDMS) relates to fasting glucose status categorized as:

Normal: < 100 mg/dL
Impaired: 100–125 mg/dL
Diabetic: ≥ 126 mg/dL
All analyses incorporate NHANES complex survey design features:

Stratification: SDMVSTRA
Primary Sampling Units: SDMVPSU
MEC Exam Weights: WTMEC2YR
Using the NHANES survey design ensures nationally representative and valid population estimates.

# Repository Structure
. ├── R/ │ └── nhanes_glucose_vitd_assumptions_first.R │ ├── output/ │ ├── model_results_logistic.txt │ └── model_results_ordinal.txt │ ├── data/ │ └── nhanes_all.csv (optional, not included) │ └── README.md

# Methods Summary
## A. Data Preparation
Created fasting glucose categories (Normal, Impaired, Diabetic)
Recoded predictors including vitamin D, age, sex, and race
Removed missing values only for relevant models
## B. Survey Design
NHANES survey design was defined as:

svydesign(
  id      = ~SDMVPSU,
  strata  = ~SDMVSTRA,
  weights = ~WTMEC2YR,
  nest    = TRUE,
  data    = df
)

---

## **C. Assumption Checks**
### 1. Linearity in the Logit
Interaction terms tested:

vitd * log(vitd)

age * log(age)

Non-significant interaction terms indicate the linearity assumption was not violated.

### 2. Multicollinearity
Assessed using Variance Inflation Factors (VIF).
All predictors showed acceptable VIF values (VIF < 3).

---

### **D. Statistical Models**
1. Binary Logistic Regression
Two survey-weighted models:

Model 1: Normal (0) vs Impaired/Diabetic (1)

Model 2: Diabetic (1) vs Normal/Impaired (0)

2. Ordinal Logistic Regression
Proportional odds model:

r
Copy code
svyolr(glu_cat ~ vitd + age + sex + race, design = nhanes_svy)
Files Included
R/nhanes_glucose_vitd_assumptions_first.R
Contains:

Data preparation

Survey design setup

Linearity tests

VIF analysis

Logistic regression models

Ordinal regression model

output/model_results_logistic.txt
Results from the two binary logistic regression models.

output/model_results_ordinal.txt
Results from the survey-weighted ordinal regression model.

Running the Analysis
Required R packages

install.packages(c("tidyverse", "survey", "car"))
Run the full analysis

source("R/nhanes_glucose_vitd_assumptions_first.R")
All model outputs will be saved automatically in the output/ directory.

---

# **NHANES Reference**
National Health and Nutrition Examination Survey (NHANES)
Centers for Disease Control and Prevention (CDC)
https://www.cdc.gov/nchs/nhanes.htm

---

# **Author**
Abrham Azale, MD, MPH(c)
Washington University in St. Louis
2025




