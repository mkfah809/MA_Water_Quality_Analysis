# Milestone 3: Feature Engineering and Baseline Model Development
**Author:** Mina Karam Nemr Fahmy  
**Course:** CS 539: Machine Learning  
**Institution:** Worcester Polytechnic Institute (WPI)

---

## 1. Objective
The goal of Milestone 3 was to transition from data exploration to predictive modeling. This involved preparing a finalized feature set, addressing missing data through imputation, and establishing a baseline performance using Logistic Regression to evaluate the initial feasibility of the research question.

---

## 2. Feature Engineering and Data Preparation

### 2.1 Feature Selection
Based on the EDA from Milestones 1 & 2, five core predictors were selected for the `HighRisk` target variable:
* **Numeric:** `Temp_C`, `nDEPTH`
* **Categorical:** `Season`, `Watershed`, `FLOWSTAT`

### 2.2 Data Cleaning & Imputation
To maintain the integrity of the 10,362-sample dataset, a simple feature-wise imputation strategy was applied:
* **Median Imputation:** Applied to `Temp_C` and `nDEPTH` to handle missing values without introducing outliers.
* **Filtering:** Dropped rows with missing `Season` values, as seasonality is a mechanical driver of dissolved oxygen levels.

### 2.3 Categorical Encoding & Scaling
* **One-Hot Encoding:** Categorical features (`Season`, `Watershed`, `FLOWSTAT`) were converted into 41 dummy variables to allow the model to interpret geographical and temporal clusters.
* **Standard Scaling:** `Temp_C` and `nDEPTH` were scaled using `StandardScaler` to ensure the varying magnitudes of temperature and depth did not bias the model coefficients.
* **Interaction Feature:** A custom feature, `Season_Summer_Temp_C_Interaction`, was created to capture the compounded risk of high temperatures specifically during the summer months.

---

## 3. Baseline Model Results: Logistic Regression

A **Logistic Regression** model with `class_weight='balanced'` was trained using 5-fold Stratified Cross-Validation to establish a performance floor.

### 3.1 Standard Threshold (0.5) Performance
| Metric | Result | Target |
| :--- | :--- | :--- |
| **Average Precision** | **19%** | 80% (Not Met) |
| **Average Recall** | **72%** | 75% (Not Met) |

### 3.2 Threshold Optimization
By lowering the decision threshold to **0.32**, the model was tuned to prioritize the detection of critical events:
* **Adjusted Precision:** 10%
* **Adjusted Recall:** 77% (Target Met)

---

## 4. Key Findings and Implications
* **Class Imbalance Impact:** The baseline model confirmed that the extreme rarity of "High-Risk" events (4.7%) makes achieving high precision exceptionally difficult for linear models.
* **Multicollinearity:** A Variance Inflation Factor (VIF) analysis revealed high correlation among watershed variables. While this creates instability in linear coefficients, the features were retained due to their practical utility for state agency field response.
* **Transition to Non-Linearity:** The inability of Logistic Regression to meet the dual 80% precision/75% recall target justified the move toward the **XGBoost** ensemble method for the final project phase.

---
