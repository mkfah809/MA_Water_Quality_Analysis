# MA Water Quality Analysis: Predicting High-Risk Dissolved Oxygen Events

This repository contains a machine learning project dedicated to predicting dangerous drops in river oxygen levels across Massachusetts. The primary goal is to provide state agencies with a proactive tool to prevent fish kills and protect aquatic ecosystems.

---

## Problem Statement
Massachusetts faces significant ecological challenges when dissolved oxygen (DO) levels drop below critical thresholds—specifically during heatwaves and low-flow periods. Current management is often reactive, occurring after fish die-offs have already begun.

### Research Goals
* **Prediction:** Can we predict if a water sample falls into **High-Risk status ($DO < 3\text{ mg/L}$)** using features like temperature, season, and watershed location?
* **Target Metrics:** Achieve at least **80% precision** and **75% recall** to ensure interventions are both accurate and comprehensive.

---

## Data & Process

### 1. Dataset
The analysis utilizes a relational warehouse of water quality data, focusing on:
* **Physical Measurements:** Dissolved Oxygen (mg/L), Temperature (°C), and Depth.
* **Contextual Data:** Watershed location, flow status, and seasonal timestamps.

### 2. Exploratory Data Analysis (EDA)
* **Correlation:** Identified a moderate negative correlation (**-0.19**) between temperature and DO, confirming that rising heat is a primary driver of oxygen crises.
* **Imbalance:** Discovered a significant class imbalance, where "High-Risk" events represent only **4.7%** of the total dataset.

### 3. Modeling Workflow
* **Preprocessing:** Handled missing values via median imputation, scaled numeric features using `StandardScaler`, and encoded categorical variables (Watershed, Season, Flow Status) using one-hot encoding.
* **Baseline:** Started with **Logistic Regression** using balanced class weights to address the data imbalance.
* **Advanced Modeling:** Employed **XGBoost** to better handle non-linear relationships and multicollinearity among geographic features.
* **Threshold Tuning:** Adjusted decision thresholds (Optimal: 0.17 for XGBoost) to prioritize **Recall**, ensuring the model catches as many critical events as possible.

---

## Results

### Model Performance
While the rarity of the minority class made the precision target difficult to reach, the model provides strong utility in identifying potential crisis zones.

| Model | Recall (High-Risk) | Precision (High-Risk) |
| :--- | :---: | :---: |
| **Logistic Regression** | 72% | 19% |
| **XGBoost (Optimized)** | **76%** | **16%** |

### Key Findings
* **Success on Recall:** The **75% Recall** goal was successfully met, meaning the system is effective at "ringing the alarm" for the majority of oxygen crises.
* **Precision Trade-off:** Reaching 80% precision remains a challenge due to the high frequency of false positives—a common result when modeling extremely rare events.
* **Top Predictors:** Temperature, Season (Summer), and specific locations (notably the **Ipswich** and **Shawsheen** watersheds) emerged as the strongest indicators of high-risk status.

---

## Repository Structure
* `wqdiscreteprobedata.csv`: The core dataset used for training and analysis.
* `MA_Water_Quality_Report.ipynb`: The complete end-to-end Python notebook containing data cleaning, EDA, and model evaluation.

---
*Developed as part of the [MA Water Quality Analysis](https://github.com/mkfah809/MA_Water_Quality_Analysis) project.*
