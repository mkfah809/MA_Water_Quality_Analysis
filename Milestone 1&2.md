
# Milestone 1 & 2: Problem Definition and Exploratory Data Analysis (EDA)
**Author:** Mina Karam Nemr Fahmy  
**Course:** CS 539: Machine Learning  
**Institution:** Worcester Polytechnic Institute (WPI)

---

## 1. Problem Statement and Motivation
The primary objective of this phase was to define a predictive framework for preventing aquatic ecosystem collapse in Massachusetts. The focus is on **Dissolved Oxygen (DO)**, where levels below **3 mg/L** represent a "High-Risk" state capable of causing mass fish die-offs.

**Research Question:**
> Can environmental factors like temperature, season, and watershed location predict a "High-Risk" DO status with high precision and recall?

---

## 2. Dataset Characterization
The analysis utilized a relational warehouse of water quality measurements sourced from the [Massachusetts Water Quality Repository](https://github.com/mkfah809/MA_Water_Quality_Analysis/main/wqdiscreteprobedata.csv).

* **Total Observations:** 10,362 samples.
* **Target Variable:** `HighRisk` (Binary: 1 if DO < 3 mg/L, 0 otherwise).
* **Key Discovery:** The dataset exhibits extreme class imbalance. Only **4.7% (490 samples)** are classified as "Critical/High-Risk," while **89.3% (9,256 samples)** are "Safe."

---

## 3. EDA Results and Technical Insights

### 3.1 The Temperature-Oxygen Inverse Correlation
The EDA confirmed a moderate negative correlation (**-0.19**) between `Temp_C` and `DO_mg_L`. 
* **Result:** High-Risk events are significantly more frequent as water temperatures rise, aligning with the scientific principle that warmer water holds less dissolved gas.
* **Tipping Point:** Most critical events were clustered at temperatures exceeding 20°C.

### 3.2 Geographical Risk Hotspots
By analyzing DO distribution across different regions, the EDA identified specific "hotspots" for oxygen depletion.
* **Finding:** The **Ipswich** and **Shawsheen** watersheds showed a disproportionately higher frequency of critical DO levels compared to more stable regions like the Blackstone or Connecticut watersheds.

### 3.3 Multicollinearity and Feature Stability
A **Variance Inflation Factor (VIF)** analysis was performed to evaluate the relationships between predictors.
* **Result:** While watershed variables showed high multicollinearity, they were retained for their "Practical Utility." For state agencies, the watershed name is the mechanical necessity required to deploy field resources.

### 3.4 Seasonal and Flow Patterns
* **Seasonality:** High-risk events were almost exclusively a Summer and early Fall phenomenon.
* **Flow Status:** "Low" and "Stagnant" flow conditions were frequently associated with the "Critical" DO category, providing a secondary indicator for model training.

---

## 4. Conclusion for Modeling Strategy
The results of Milestones 1 & 2 directly informed the following project decisions:
1.  **Model Choice:** Because the EDA revealed non-linear relationships and high class imbalance, the project moved toward **XGBoost** with `scale_pos_weight`.
2.  **Evaluation Metric:** Due to the 4.7% positive class frequency, **Recall** was prioritized over Accuracy to ensure critical events are not missed.
