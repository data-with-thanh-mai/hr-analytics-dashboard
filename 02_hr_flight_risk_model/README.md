
# Flight Risk Detection: An Early Warning System for HR

*This directory contains Phase 2 of the End-to-End HR Analytics: From Descriptive Dashboard to Predictive AI. If you are looking for the overall business context or the EDA/Tableau Dashboard, please refer to the [Master README](../README.md) or [Phase 1: HR Dashboard](../01_hr_dashboard_analysis/).*


## 🛠 Tech Stack
* **Environment:** Jupyter Notebook
* **Language:** Python 3.8+
* **Data Processing & ML:** Pandas, Scikit-learn, XGBoost, imbalanced-learn (SMOTE)
* **Model Explainability:** SHAP


> 👉 All technical implementations, EDA, and model tuning can be found in the **[HR_Flight_Risk_Prediction.ipynb](02_hr_flight_risk_model/HR_Flight_Risk_Prediction.ipynb)** notebook.

## Table of Contents
- [1. Context & Business Problem](#1-context--business-problem)
- [2. The Proposed Solution](#2-the-proposed-solution)
- [3. Strategy & Business ROI](#3-strategy--business-roi)
- [4. Technical Approach & Final Results](#4-technical-approach--final-results)
- [5. The Actionable Output](#5-the-actionable-output)
## 1. Context & Business Problem

**The Pain Point:** Our HR data (>8,000 employees) shows an annual turnover rate of 2.45% (2024). While seemingly stable, losing ~201 employees yearly creates a massive financial loophole. With the average cost-per-hire at $4,700 (SHRM Benchmark), the **Cost of Talent Drain is ~$1 Million/year**.

**Current HR Method:** Highly reactive. HR identifies flight risks through lagging indicators (frequent leaves, low engagement). By the time these signs are visible, employees often have competing offers, making retention efforts fail.

---

## 2. The Proposed Solution

We built an **AI Early Warning System** using Machine Learning to shift HR from a guessing game to a proactive, data-driven strategy.

| Feature | Traditional HR (Current State) | AI Early Warning System (Proposed) |
| :--- | :--- | :--- |
| **Approach** | **Reactive:** Acts only after red flags are obvious. | **Proactive:** Flags potential risks 30-60 days in advance. |
| **Based On** | Manager's intuition and guesswork. | Objective data (Salary, Age, Tenure, Performance). |
| **Scalability** | Low: Limited to what managers can observe. | **High:** Scans 8,000+ employees in seconds. |
| **Insights** | Sees only surface-level issues. | **SHAP Analysis:** Pinpoints the exact factors driving the risk. |

---

## 3. Strategy & Business ROI

We applied a **Cost Matrix strategy** to 8,950 historical HR records (1,000 attrition cases):
* **Cost of Missing a risk:** ~$5,000/person (replacement costs).
* **Cost of False Alarm:** ~$500/person (HR time spent on 1-on-1s).

Since missing a flight risk is 10x more costly than a false alarm, our model optimizes for **Recall** and **F1-Score** to capture as many genuine risks as possible without causing "Alert Fatigue" for HR.

> 👉 **Business Target:** Retaining just 20% of the high-risk core talent (about 40 people/year) will directly save the company **$188,000 annually**.

---

## 4. Technical Approach & Final Results

* **Imbalanced Data:** Applied SMOTE to oversample the minority class (2.45% attrition rate) to prevent majority bias.
* **Algorithm:** Benchmarked Logistic Regression, Decision Tree, and XGBoost. **XGBoost** was selected as the champion model (Baseline AUC-ROC: 0.91).
* **Fine-Tuning:** Used **Grid Search** to optimize hyperparameters for the F1-Score, maximizing early detection capabilities. (Optimal config: `learning_rate: 0.1`, `max_depth: 6`, `scale_pos_weight: 3`).

**Performance Metrics (Test Set):**

| Metric | Score | What this means for HR |
| :--- | :--- | :--- |
| **Recall** | **73%** | Successfully identifies 73% of all employees planning to leave. |
| **Precision** | **62%** | When the AI issues a warning, there is a 62% probability it is completely accurate. |
| **F1-Score** | **0.67** | An optimal balance between missed risks and false warnings. |
| **Overall Accuracy** | **92%** | Correctly classifies 92% of all employee profiles. |

---



## 5. The Actionable Output

The final deliverable of this project is an automated CSV file. For each at-risk employee, the report provides the predicted risk score, the top driving factors (explained by SHAP), and concrete recommendations for HR. 


*Snapshot from the automated report ([HR_Retention_Alerts_2026_06.csv](data/HR_Retention_Alerts_2026_06.csv):*

| Employee ID | Risk Score | Top Driving Factors (via SHAP) | Actionable Advice |
| :--- | :--- | :--- | :--- |
| **00-66691738** | 91.6% | 1. Salary vs. Edu Average <br> 2. Dept: Sales <br> 3. Dept: Operations | Review salary compared to their degree and department avg. |
| **00-49684512** | 91.0% | 1. Location (New York) <br> 2. Base Salary <br> 3. Location (Michigan) | Check local market competitiveness in New York. |
| **00-19976458** | 90.4% | 1. Tenure-to-Age Ratio <br> 2. Location (New York) <br> 3. Education Level | Discuss career progression path; Identify if they feel "stuck". |

*(Note: This roster is designed to be exported monthly or integrated directly into BI Web Dashboards—such as Streamlit, Tableau or Power BI—allowing HR Leaders to manage retention proactively).*
