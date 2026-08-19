# Commercial Bank Loan Portfolio Risk Visualization Dashboard

## Executive Project Overview
This repository delivers an end-to-end credit risk analytics framework designed to simulate an institutional banking environment. By combining an automated Python-driven data pipeline, a Logistic Regression machine learning classification engine, and an interactive Power BI dashboard, this project enables financial risk managers to monitor portfolio health, track non-performing assets (NPAs), and assess forward-looking default probabilities in real time.

### Business Value & Value Proposition
In commercial banking, unmitigated default risk erodes capital reserves. This framework bridges the gap between raw ledger data and executive decision-making. By calculating account-specific risk probabilities, it enables credit teams to proactively identify high-risk segments, adjust capital allocations, optimize lending criteria, and mitigate potential write-offs before they impact the balance sheet.

---

## Tech Stack & End-to-End Pipeline Architecture
The system uses a decoupled, three-tier architecture mirroring production-level cloud deployments:
[ Raw Loan Records ] ➔ [ Python ML Engine (Colab) ] ➔ [ Scored Dataset (CSV) ] ➔ [ Power BI DAX Engine ] ➔ [ Executive View ]

1. **Staging & Engineering Layer (Python):** Cleans raw borrower ledgers and handles algorithmic preprocessing.
2. **Modeling & Inference Layer (scikit-learn):** Trains a balanced Logistic Regression classifier to calculate continuous credit risk scores.
3. **Semantic Modeling & Visualization Layer (Power BI):** Ingests model outputs, processes business logic via custom DAX measures, and renders interactive analytical reports.

---

## Predictive Engineering & Modeling Framework
The predictive core processes active borrower risk profiles to estimate the statistical likelihood of account failure. 

### Feature Architecture
The machine learning pipeline evaluates five primary credit risk indicators:
*   `borrower_income`: Continuous financial capacity indicator.
*   `dti_ratio`: Total monthly debt obligations divided by gross monthly income.
*   `interest_rate`: Asset pricing reflecting baseline borrower risk underwriting tier.
*   `credit_utilization`: Active revolving credit card balance usage percentages.
*   `delinquencies_6m`: Historical short-term payment failure count (behavioral risk proxy).

### Model Execution Metrics
*   **Algorithm Type:** Logistic Regression Classifier (`class_weight='balanced'`)
*   **Data Scaffolding:** Robust Z-score feature scaling via `StandardScaler` to handle cross-feature scale variances.
*   **Performance Score:** Achieved a validation **ROC-AUC score of 0.7135**, confirming solid predictive power and balanced classification thresholds without overfitting.

---

## Power BI Implementation & DAX Architecture
Downstream metrics are generated dynamically within Power BI using specialized financial business logic structured via Data Analysis Expressions (DAX):

### 1. Total Active Portfolio Exposure
Tracks the absolute dollar volume of active credit outstanding across the institution.
```dax
Total Exposure = SUM('processed_loans'[current_balance])
```

### 2. Weighted Average Probability of Default (WA PD)
Prevents large safe accounts from skewing baseline averages by weighting account risks directly against outstanding asset sizes.
```dax
WA PD = 
DIVIDE(
    SUMX('processed_loans', 'processed_loans'[current_balance] * 'processed_loans'[probability_of_default]),
    [Total Exposure],
    0
)
```

### 3. Projected Portfolio Expected Loss (EL)
Estimates forward-looking credit losses assuming an industry-standard institutional
**Loss Given Default (LGD) baseline of 45%**.
```dax
Expected Loss = 
SUMX(
    'processed_loans', 
    'processed_loans'[current_balance] * 'processed_loans'[probability_of_default] * 0.45
)
```

---

## Executive Dashboard Interface Map
The Power BI user interface uses a structured grid layout designed for corporate stakeholders:

## Executive Dashboard Interface Map
The Power BI user interface uses a structured grid layout designed for corporate stakeholders:

*   *Executive Metrics Strip (Top):* High-level card visuals delivering rapid readouts of *Total Exposure*, *WA PD*, and **Expected Loss* to gauge portfolio health instantly.
*   *Behavioral Risk Breakdown (Bottom Left):* A categorical clustered column chart matching past borrower delinquency occurrences with projected asset loss numbers.
*   *Capacity Distribution Trend (Bottom Right):* A continuous area chart plotting borrower income segments against default probability slopes, smoothing raw data points into distinct \$20,000 income brackets.own (Bottom Left):
