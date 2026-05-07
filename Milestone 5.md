# Predicting Dissolved Oxygen Crises in Massachusetts Final Report
**Author:** Mina K. Fahmy  
**Institution:** Worcester Polytechnic Institute (WPI)

---

## 1. Abstract
This project addresses the critical environmental challenge of predicting **"High-Risk" Dissolved Oxygen (DO) levels (< 3 mg/L)** in Massachusetts rivers. Utilizing a dataset of **10,362 observations (2005–2020)** from the MassDEP, I developed a predictive framework using **Logistic Regression, XGBoost, and Random Forest**. To mitigate a severe **4.7% class imbalance**, I implemented **SMOTE** and decision threshold optimization. The final **Random Forest** model achieved a **Recall of 75%** and an **AUC of 0.85**, meeting the operational targets for an early-warning system designed to prevent mass fish die-offs and aquatic ecosystem collapse.

---

## 2. Overview and Motivation
Massachusetts river systems are highly sensitive to thermal stress. When water temperatures rise, oxygen solubility decreases, often leading to "crises" where DO levels drop below the survival threshold for aquatic life. Traditional monitoring is reactive—state agencies often only discover these events after fish die-offs occur. 

The motivation for this project is to provide a **proactive early-warning tool**. By predicting high-risk events based on temperature, flow, and location, state agencies can prioritize limited monitoring resources and implement heat-resilient infrastructure in the most vulnerable watersheds.

---

## 3. Related Work
This project was inspired by MassDEP and the data storytelling approaches used by water-quality-monitoring-program. The choice to prioritize **Recall** over Precision mirrors established ecological safety protocols where the "cost of a false negative" (an undetected fish kill) is significantly higher than the "cost of a false alarm" (sending a technician to a healthy site). The implementation of **SMOTE** follows standard industry practices for imbalanced "needle-in-a-haystack" problems.

---

## 4. Project Description
### 4.1 Research Question
Can we identify predictable patterns in Dissolved Oxygen depletion using environmental factors, and can a machine learning model catch 75% of these crises using imbalanced field data?

### 4.2 Data Documentation
* **Source:** [MassDEP Watershed Planning Program](https://www.mass.gov/guides/water-quality-monitoring-program-data).
* **Observations:** 10,362 unique field measurements.
* **Timeframe:** 2005–2020 (16 years of longitudinal data).
* **Geographical Scope:** Major Massachusetts watersheds, including the Ipswich, Shawsheen, and Blackstone.
* **Key Variables:** `DO_mgL` (Target), `Temp_C`, `Season`, `Watershed`, `nDEPTH`, and `FLOWSTAT`.

---

## 5. Methods and Methodology
### 5.1 Preprocessing and Feature Engineering
* **Imputation:** Applied **Median Imputation** to `Temp_C` and `nDEPTH` to handle missing values while remaining robust to field sensor outliers.
* **Interaction Features:** Created a **Summer-Temperature Interaction term** ($Temp\_C \times Season\_Summer$) to capture the non-linear oxygen crash that occurs during summer stagnation.
* **Multicollinearity Justification:** A **VIF analysis** detected high correlation among watershed dummies. However, these were **retained** because knowing the specific watershed is a "mechanical necessity" for actionable state intervention.

### 5.2 Model Implementation
I utilized **5-Fold Stratified Cross-Validation** to ensure class ratios remained consistent across folds.
1. **Logistic Regression:** Served as the baseline.
2. **XGBoost:** Implemented with `scale_pos_weight` to penalize minority class errors.
3. **Random Forest:** Utilized as a sophisticated ensemble to capture complex feature interactions.
4. **SMOTE:** Applied to the training set to balance the 4.7% minority class to a 50/50 ratio.

---

## 6. Final Analysis and Comparison
All models were tuned to find the "Optimal Threshold" that satisfies the **75% Recall** requirement.

### 6.1 Formal Model Comparison Table
| Model | Optimal Threshold | Precision | Recall | F1-Score | AUC |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Logistic Regression | 0.32 | 10% | 77% | 0.18 | 0.81 |
| XGBoost | 0.17 | 16% | 76% | 0.26 | 0.78 |
| **Random Forest** | **0.04** | **18%** | **75%** | **0.29** | **0.85** |

### 6.2 Key Findings
* **The Precision-Recall Trade-off:** As predicted, precision remains low (18%) when recall is pushed to 75%. This is the "operational reality" of working with extreme class imbalance.
* **Feature Importance:** [SHAP Values](https://colab.research.google.com/drive/1ncjSpEW8SUxfsPX8yuWSCIkyhIulspl1#scrollTo=680) confirm that **Temperature** is the primary driver of risk, followed closely by the **Ipswich** watershed geography.

---

## 7. Technical Discussion
Multicollinearity (high VIF) was present in our location-based features. While this would destabilize a simple linear model, our use of **Random Forest** and **XGBoost** mitigated this issue. Tree-based ensembles are inherently robust to collinearity because they choose features that provide the most information gain at each split, allowing us to keep critical geographical data for the end-user without sacrificing predictive power.

---

## 8. Conclusion and Recommendations:
Predicting low DO matters because it is the difference between a thriving river and a toxic one. While our model has a high false-alarm rate (18% precision), it captures **3 out of every 4 oxygen crises**. 

**Recommendations:**
1. **Targeted Monitoring:** MassDEP should deploy mobile sensors to the **Ipswich and Shawsheen** watersheds whenever water temperatures are predicted to rise above the identified 20°C tipping point.
2. **Infrastructure:** Prioritize "green infrastructure" (shading trees and runoff bioswales) in these hotspots to lower water temperatures.
3. **Future Work:** Integrating nutrient data (Nitrates/Phosphates) would likely improve precision by identifying algal-driven oxygen crashes.

---
