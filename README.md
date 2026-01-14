**Customer Churn Prediction- Trading Platform (Write-Up)**

Srijha Thammareddy

**Colab link**: [Customer\_churn\_prediction\_TradingPlatform\_Tech68.ipynb](https://colab.research.google.com/drive/1sDLaC-IRTofSP_Yz3opzWDeFUsNn1kx0?usp=sharing)

## **1\) Overview**

Goal: Predict which users are likely to churn, enabling proactive retention actions.   
Formulated churn label, prepared user and equity features, and evaluated multiple models.

## **2\) Data**

•  Observation window: Aug 16, 2016 \- Aug 18, 2017\.  
•  Equity\_value\_data: user-day level close\_equity.  
• User\_features: risk\_tolerance, investment\_experience, liquidity\_needs, platform (Android/iOS/both), instrument\_type\_first\_traded, time\_horizon, time\_spent, first\_deposit\_amount, etc.

Note: Given data does not have Churn labels, so I created a churn label with below definition.

## **3\) Churn Label Definition**

• Define a day as low-equity if close\_equity \< $10.  
• Compute a rolling 28‑day sum of low‑equity per user.  
• Label user as churn=1 if any 28‑day window has low‑equity on all 28 days; else churn=0.

## **4\) Models**

• Logistic Regression  
• LightGBM  
• XGBosst

## **5\) Methodology**

**5.1 Data Preparation**

• Merged equity time series with user attributes at the user level.  
• Handled missing values; standardized types.  
• Encoded categoricals: one‑hot for Logistic regression; integer codes for tree models.  
• Numeric feature scaling for Logistic Regression.

**5.2 Exploratory Data Analysis (EDA)**

• Checked distributions, correlations, and churn rates by segments.  
• Reviewed outliers and class balance.  
• Confirmed no obvious leakage in core features.

**5.3 Modeling & Evaluation**

• Models: Logistic Regression (baseline), XGBoost, LightGBM.  
• Metrics: ROC‑AUC as primary; F1‑optimized threshold for operating point.  
• Validation: Hold‑out test set; early stopping for gradient boosting.

## **6\) Results (Test Set)**

**6.1 ROC‑AUC & Best F1 Threshold**

| Model | ROC‑AUC | Best F1 Threshold |
| :---- | :---- | :---- |
| Logistic Regression | 0.797 | 0.455 |
| XGBoost | 0.817 | 0.465 |
| LightGBM | 0.815 | 0.455 |

   
**6.2 Confusion Matrices at Best F1 Threshold**

**Logistic Regression**

|   | Pred: Negative | Pred: Positive |
| :---- | :---- | :---- |
| **Actual: Negative** | 460 | 150 |
| **Actual: Positive** | 134 | 373 |

**LightGBM**

|   | Pred: Negative | Pred: Positive |
| :---- | :---- | :---- |
| **Actual: Negative** | 460 | 150 |
| **Actual: Positive** | 134 | 373 |

**XGBoost**

|   | Pred: Negative | Pred: Positive |
| :---- | :---- | :---- |
| **Actual: Negative** | 470 | 140 |
| **Actual: Positive** | 132 | 375 |

**Summary**: XGBoost (AUC: 0.817) and LightGBM (AUC: 0.815) perform similarly and both exceed the Logistic Regression baseline (AUC: 0.797). At their F1‑optimized thresholds, XGBoost shows slightly fewer false positives/negatives.

## **7\) Conclusions**

• A conservative churn label based on consecutive low‑equity days is intuitive to  model.  
• Gradient boosting (XGBoost/LightGBM) slightly outperforms the linear baseline while handling mixed feature types well.  
• Several categorical traits and simple behavioral/financial features add meaningful signal
