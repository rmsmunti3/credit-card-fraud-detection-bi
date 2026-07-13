# Credit Card Fraud Detection - End-to-End BI & Machine Learning Case Study

A comprehensive Business Intelligence (BI) and Machine Learning (ML) project developed as part of the **CCDS313 - Business Intelligence** course at the **University of Jeddah**.

This repository showcases an end-to-end analytics project, transitioning from a conceptual design to a fully deployed dashboard and machine learning pipeline. It addresses the highly complex "needle in a haystack" problem of credit card fraud detection.

---

## Project Structure & Evolutionary Phases
The project was executed in two progressive stages to build a solid BI framework:

1. **Phase 1 (Assignment 1 - Conception):** Focused on the theoretical framework, defining the problem statement, framing targeted analytical questions, and designing the initial concept of the fraud detection ecosystem.
2. **Phase 2 (Assignment 2 - Implementation & Deep Dive):** Translated our conceptual design into actionable solutions. This phase involved conducting ETL processes, developing the interactive Power BI dashboard, building the predictive model in RapidMiner, and setting up prescriptive decision rules.

---

## My Individual Contribution
While this was a collaborative group project, my primary focus and ownership was centered on the **Descriptive Analytics, ETL, & Visualization Phase**:
* **ETL & Data Preprocessing:** Cleaned and transformed raw European cardholder transaction data (284,807 records) within **Power Query**. Optimized currency types, resolved field naming conventions for better business alignment, and created explicit text labels (translating Class 0/1 to "Legitimate" and "Fraud").
* **Dashboard Development (Power BI):** Designed and structured the entire interactive Power BI dashboard layout to deliver actionable insights on transaction counts, total fraud losses, behavioral comparisons, and temporal spikes.
* **Synthesis & Business Rules:** Collaborated closely with my teammates to bridge the gap between descriptive statistics and the predictive/prescriptive models in **RapidMiner**.

---

## Technical Architecture & Dataset
* **Dataset:** 284,807 transactions spanning 48 hours, featuring 31 numerical features (Time, Amount, Class, and PCA-transformed features V1-V28 for privacy)[cite: 1].
* **Tools:** Power BI (Power Query & DAX), RapidMiner Studio (Random Forest Classifier & Custom Decision Logic).

---

## Detailed Analytics Findings & Insights

### 📊 Phase 1: Descriptive Analytics Insights (Power BI)
Our descriptive phase uncovered critical behavioral and structural patterns within the transactional history:

1. **The Magnitude of Fraud:** 
   * Out of 284,807 transactions, only **492 (0.17%)** were fraudulent.
   * Despite this tiny percentage, the total financial loss caused by these 492 transactions was **$60.13K**, proving that fraud is an expensive threat that heavily justifies the investment in automated ML pipelines.
2. **Behavioral Divergence (Transaction Size):**
   * **Legitimate Transactions:** Average amount was **$88.29**.
   * **Fraudulent Transactions:** Average amount was **$122.21**.
   * *Insight:* Fraudulent activities tend to target higher-than-average amounts. This behavioral difference proves that the `Amount` field is a high-value predictor for classification algorithms[cite: 1].
3. **Temporal Fraud Spikes (Time Window Analysis):**
   * Plotting transaction frequency over time showed that fraud attempts do not occur in a continuous, smooth flow. Instead, they occur in **abrupt, anomalous spikes**.
   * *Insight:* These sudden spikes indicate highly automated, scripted attacks rather than manual attempts. Banks can leverage this to schedule high-alert monitoring periods and automatically scale fraud-checking computing resources during peak transaction hours.

---

### 🤖 Phase 2: Predictive Analytics & The Imbalance Challenge (RapidMiner)
We split the dataset into a **70% Training / 30% Testing** split and trained a **Random Forest Classifier**.


* **The Paradox of High Accuracy:** 
  Our model achieved an overall accuracy of **99.63%**.
* **The Recall Breakdown (0% Recall):**
  Despite the outstanding accuracy, the confusion matrix revealed a critical real-world problem: the model achieved **0% Recall** (it failed to detect any fraudulent transactions, classifying all test cases as "Safe").
* **Key Learning Outcome:**
  This performance gap occurred because of the **extreme class imbalance** (0.17% minority class). Because legitimate cases dominate the dataset, standard classifiers optimize overall accuracy by simply guessing "Legitimate" every time. 
  * *Next-Step Recommendation:* In a production environment, applying advanced preprocessing techniques such as **SMOTE** (Synthetic Minority Over-sampling Technique) or cost-sensitive learning is absolutely essential to force the model to capture the minority class.

---

### ⚙️ Phase 3: Prescriptive Analytics & Decision Logic
To convert predictions into concrete business actions, we translated our ML model's confidence scores into an automated decision engine:

| Risk Level | Confidence Threshold | Action Taken | Business Justification |
| :--- | :--- | :--- | :--- |
| **High Risk** | Confidence > 90% | **Auto-Block & Notify** | Instantly blocks the transaction and sends an SMS/App notification. Prevents immediate loss with zero delay. |
| **Medium Risk** | Confidence 50% - 90% | **MFA & Manual Review** | Triggers Multi-Factor Authentication (MFA). If verified, the transaction proceeds; if not, it is flagged for analyst review. Balances security and customer friction |
| **Low Risk** | Confidence < 50% | **Auto-Approve** | Automatically approves the transaction. Ensures a frictionless checkout experience for genuine users. |

---

## Key Takeaways & Recommendations
1. **Never Trust Accuracy Alone:** For highly imbalanced datasets like fraud detection, metrics like **Precision, Recall, and F1-Score** are far more critical than raw Accuracy.
2. **Dynamic Retraining:** Fraud tactics evolve constantly. Models must be continuously retrained with fresh transaction streams to prevent conceptual drift.
3. **Data-Driven Strategy:** Combining descriptive visualizations with prescriptive rule engines reduces human intervention costs, focusing expensive manual review efforts only on ambiguous, medium-risk transactions.
