# Final Analysis Report: Predicting Dissolved Oxygen Crises in Massachusetts
**Author:** Mina Karam Nemr Fahmy  
**Course:** CS 539: Machine Learning  
**Institution:** Worcester Polytechnic Institute (WPI)  

---

## 1. Project Overview & Summary
This project addresses a critical environmental challenge: preventing fish kills and protecting aquatic ecosystems in Massachusetts. By leveraging a historical dataset of **10,362 water quality samples**, I developed a predictive framework to identify **"High-Risk" Dissolved Oxygen (DO) events**, defined as levels falling below **3 mg/L**. 

The final model, built using **XGBoost**, successfully achieved a **Recall of 76%**, meeting the core project requirement for identifying critical oxygen depletion events. While the precision remains low due to extreme class imbalance (only 4.7% of samples are High-Risk), the model serves as an effective early-warning tool for state agencies to prioritize monitoring in vulnerable watersheds like the **Ipswich** and **Shawsheen**.

---

## 2. Research Intent
Massachusetts rivers are increasingly vulnerable to heatwaves and urban runoff. Traditional reactive monitoring, discovering a problem only after a fish die-off is insufficient. 

**Research Question:** Can machine learning utilize temperature, seasonal trends, and geographical location to predict High-Risk DO status with at least 75% recall, and which features drive these critical events?

**Aims:**
* Identify the "tipping point" where water temperature causes oxygen crashes.
* Evaluate the effectiveness of gradient boosting over linear models for imbalanced environmental data.
* Pinpoint specific watersheds that require immediate green-infrastructure investment.

---

## 3. Dataset Description
The dataset was sourced from the [Massachusetts Water Quality Repository](https://github.com/mkfah809/MA_Water_Quality_Analysis). 
* **Contents:** 10,362 field measurements including Dissolved Oxygen (mg/L), Temperature (C), Watershed ID, Flow Status, and Depth.
* **Target:** `HighRisk` (Binary: 1 if DO < 3.0, 0 otherwise).
* **Preprocessing:** Missing values for `nDEPTH` and `nTEMP` were imputed with medians; categorical features (Watershed, Season) were one-hot encoded to handle non-linear geographical impacts.

---

## 4. Methods: The XGBoost Approach
Initial modeling with **Logistic Regression** struggled to balance the rare "High-Risk" events, yielding poor stability across different watersheds.

**XGBoost was implemented for its specific advantages:**
1.  **Handling Imbalance:** Utilizing `scale_pos_weight` (approx. 20.2) to penalize the misclassification of rare oxygen crises.
2.  **Tree-Based Splitting:** Effectively capturing the non-linear relationship between rising temperatures and oxygen solubility.
3.  **Threshold Tuning:** The decision threshold was shifted to **0.17** to prioritize **Recall** (catching the crisis) over **Precision** (avoiding false alarms).

---

## 5. Results and Conclusion

### 5.1 Performance Highlights
Through 5-fold Stratified Cross-Validation, the XGBoost model demonstrated robust ability to catch crises:

* **Final Recall:** 76% (Target > 75% met).
* **Final Precision:** 16%.
* **Accuracy:** 80%.

### 5.2 Key Figures & Visual Insights
* **Feature Importance:** The model identified `Temp_C` as the primary driver, followed closely by the **Ipswich** and **Shawsheen** watersheds. This confirms that local geography and thermal stress are the leading indicators of ecosystem collapse.
* **Confusion Matrix Analysis:** At the 0.17 threshold, the model correctly identified 372 out of 490 high-risk events in the total dataset.

### 5.3 Final Summary
The "so what?" of this project is clear: While the model produces false alarms, catching **76% of oxygen crises** allows environmental agencies to intervene before permanent ecological damage occurs. The XGBoost architecture proved superior to linear methods in managing the multi-collinearity of watershed data and the sparsity of critical events. 

**Recommendation:** Agencies should deploy mobile aeration units or restrict runoff in the **Ipswich** and **Shawsheen** zones whenever predicted water temperatures exceed the 1-standard deviation threshold identified in the [Process Notebook](https://colab.research.google.com/drive/1ncjSpEW8SUxfsPX8yuWSCIkyhIulspl1).

---
