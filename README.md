# ${\color{blue}\text{Customer Churn Prediction - Interconnect Telecom}}$
A machine learning system to identify customers at risk of leaving, enabling proactive retention campaigns before they churn.

🔗 Live Demo → [churn-project-rodriguez-robcar.streamlit.app](https://churn-project-rodriguez-robcar.streamlit.app/)
#

### 📌 Overview
Interconnect, a telecom operator, wants to proactively identify customers likely to cancel their service. When a customer is flagged as high-risk, the marketing team can intervene with promotional codes and tailored plan offers before they leave.
This project builds an end-to-end churn prediction pipeline — from raw customer data through feature engineering, model selection, and a deployed Streamlit app.

#

### 🗂️ Services Offered by Interconnect

Interconnect provides the following services, all of which feed into customer behavior data:

Core services

- Landline communication — phone lines with multi-line support
- Internet — via DSL (digital subscriber line) or fiber optic cable

Add-on services

- OnlineSecurity — malicious website blocker
- DeviceProtection — antivirus software
- TechSupport — dedicated technical support line
- OnlineBackup — cloud file storage and data backup
- StreamingTV / StreamingMovies — media streaming

Billing & contracts

- Monthly, 1-year, or 2-year contract options
- Multiple payment methods supported
- Electronic invoicing available

#

### 📊 Dataset
The dataset was provided as part of a data science bootcamp and contains anonymized customer behavior and subscription data across four tables.

| Table | Description |
|-------|----------|
| contract.csv | Contract and billing details |
| personal.csv | Customer demographics |
| internet.csv | Internet service subscriptions |
| phone.csv | Phone service details |

<details>
<summary><strong>Column reference (click to expand)</strong></summary>
  
- <mark>contract.csv</mark>: contract information
  - customerID, BeginDate, EndDate
  - Type - conntract duration (monthly, 1-year, 2-year)
  - PaperlessBilling, PaymentMethod
  - MonthlyCharges, TotalCharges
 
- <mark>personal.csv</mark>: client's personal information
  - customerID, gender, SeniorCitizen, Partner, Dependents
 
- <mark>internet.csv</mark>: internet services information
  - customerID, InternetService
  - OnlineSecurity, OnlineBackup, DeviceProtection
  - TechSupport, StreamingTV, StreamingMovies
 
- <mark>phone.csv</mark>: phone services information
  - customerID, MultipleLines
 
</details>

#
 
### 🚀 Workflow

1. 🔍 Data Cleaning & EDA
   - Merged four source tables on customerID
   - Imputed missing TotalCharges values with MonthlyCharges (customers with no charge history yet)
   - Explored churn distribution: 73% active / 27% churned — imbalanced dataset

  
2. 🛠 Feature Engineering
   - Tenure — computed as the number of months since contract start. For active customers (no EndDate), the dataset's latest date is used as reference.
   - Ordinal Encoder for Type
   - 0-1 Mapping for binary features
   - Scaling for Logistic Regression
   - Class weight for Random Forest (class imbalance is 73/27)
  
3. Preprocessing
   <table>
     <tr>
       <th>Feature type</th>
       <th>Strategy</th>
     </tr>
     <tr>
       <th>Binary features (Yes / No)</th>
       <th>0-1 mapping</th>
     </tr>
     <tr>
       <th>Internet Services, Payment Method</th>
       <th>One-Hot Encoding</th>
     </tr>
     <tr>
       <th>Type (contract length)</th>
       <th>Ordinal Encoding</th>
     </tr>
     <tr>
       <th>Numeric features</th>
       <th>Standard scaling (for Logistic Regression)</th>
     </tr>
     <tr>
       <th>Class Imbalance</th>
       <th>class_weight='balanced' (Random Forest</th>
     </tr>  
   </table>  
    
4. 🤖 Modeling and Evaluation
   Baseline: DummyClassifier

   Models Evaluated:
   - Logistic Regression
   - Random Forest
   - CatBoost Classifier
   - XGBoost Classifier
   - LightGBM Classifier

Metric rationale: Accuracy was deprioritized — a model predicting "no churn" for all customers would achieve 73% accuracy by default. The primary metrics are:

- Recall (churn class) — to catch as many at-risk customers as possible
- F1-score - to evaluate performance on imbalanced datasets where the positive class is rare, as it ensures both few false positives and few false negatives
- ROC-AUC — to measure overall ranking quality

#

🏆 Best Model: CatBoost Classifier

Hyperparameters: depth=4, learning_rate=0.03, l2_leaf_reg: 5

#### Test Set Metrics

| Metric | Score |
|---|---|
| ROC-AUC | **0.92** |
| Recall (churn) | **0.82** |
| F1-score (churn) | 0.71 |
| Accuracy | 0.82 |

#

### 💼 Business Context

Customer churn represents a significant threat to subscription-based revenue models. Retaining existing customers is typically more cost-effective than acquiring new ones.

By identifying at-risk customers early, Interconnect can:

- Target retention offers only to customers who need them (reducing campaign waste)
- Prioritize high-value customers for proactive outreach
- Measure and improve retention ROI over time

#

### 🛠️ Tech Stack

`Python` · `Pandas` · `Scikit-learn` · `CatBoost` · `XGBoost` · `LightGBM` · `Streamlit`

#

### 📁 Project Structure

```
├── data/
│   ├── contract.csv
│   ├── personal.csv
│   ├── internet.csv
│   └── phone.csv
├── notebooks/
│   └── churn_analysis.ipynb
├── app.py                  # Streamlit app
├── model/                  # Saved model artifacts
└── README.md
```

#

### ▶️ Run Locally

```bash
git clone https://github.com/rodriguez-robcar/customer_churn_prediction
cd customer_churn_prediction
pip install -r requirements.txt
streamlit run app.py
```
#

### Screenshot

<img width="2552" height="2065" alt="screencapture-churn-project-rodriguez-robcar-streamlit-app-2026-02-18-20_51_32" src="https://github.com/user-attachments/assets/42bb52ad-0de4-4297-a37b-a2dcaec84b19" />
