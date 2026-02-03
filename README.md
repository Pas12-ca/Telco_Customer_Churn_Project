# Customer Churn Prediction & Retention Strategy

## Project Overview
The objective of this project is to **predict customer churn** for a telecommunications company and propose actionable retention strategies. Customer churn represents a major business challenge because losing a customer directly reduces revenue, while acquiring new customers is costly.

The project leverages machine learning models to predict churn probability based on **contractual, demographic, and usage-related features** (tenure, contract type, subscribed services, monthly charges, etc.). High-risk customers identified by the model can be targeted with retention campaigns.

---

## Business Problem
- **Target variable:** `Churn` (Yes/No)
- **Business goal:** Reduce churn and maximize customer lifetime value (LTV)
- **Evaluation metric:** Prioritize **Recall** for churners, since false negatives (missing churners) have a higher business cost than false positives.

**Key Business Insights:**
- Observed churn rate: 26.5% (more than 1 in 4 customers churn)
- High-risk segments: recent customers, month-to-month contracts, high monthly charges, fiber optic internet, electronic check payments, lack of tech support, absence of online security.

---

## Data Understanding
**Dataset:**
- Customer-level data with contractual, demographic, usage, and financial features
- Target: `Churn`
- Main numerical features: `tenure`, `MonthlyCharges`, `TotalCharges`
- Categorical features: `Contract`, `PaymentMethod`, `TechSupport`, `OnlineSecurity`, etc.
- Issues handled:
  - Missing or improperly formatted `TotalCharges`
  - Class imbalance (churners < non-churners)

**Key Findings from EDA:**
- Short tenure → higher churn
- Month-to-month contracts → high churn (~43%)
- Fiber optic users → high churn (~42%)
- Customers without tech support or online security → high churn (~42–49%)
- High monthly charges correlate with increased churn

**Business levers:**
- Incentivize longer-term contracts
- Promote tech support and security services
- Encourage automatic payments
- Target high-billing fiber customers for retention campaigns

---

## Modeling Approach
**Selected Model:** `RandomForestClassifier` with an aggressive strategy to maximize Recall  
- Other models evaluated: LightGBM, Logistic Regression, CatBoost, XGBoost  
- **Metrics for RandomForest (Aggressive Strategy):**
  - Recall: 0.88 (highest among tested models)
  - Precision: 0.45 (acceptable for cost management)
  - F1-score: 0.60
- **Business Cost Analysis:**
  - Aggressive strategy reduces estimated churn cost by ~30% compared to balanced strategy

**Feature Importance Highlights:**
- `tenure`, `Contract_Two year`, `Contract_One year`
- `InternetService_Fiber optic`, `MonthlyCharges`, `TotalCharges`
- `TechSupport_Yes`, `OnlineSecurity_Yes`

---

## Retention Strategy
**Segmentation-Based Approach:**
- Each segment evaluated for expected **value saved** vs **action cost**  
- GO/NO GO decisions based on **Net Value = Value Saved – Action Cost**  
- Example action costs per segment:
  - Onboarding / Welcome Bonus: $50
  - Premium Support: $40
  - Security Bundle: $30
  - Payment Alerts / Grace Period: $15
  - Pricing Discount: $60
  - Stable customers: $0

**High-Risk Segments:**
| Segment | Churn Rate | Recommended Actions |
|---------|------------|-------------------|
| New High Risk | 56% | Reinforced onboarding, targeted calls, welcome bonus |
| Monthly No Support | 50% | Temporary premium support, annual contract upgrade, proactive communication |
| Senior No Security | 50% | Simplified security bundle, human support, non-technical communication |
| Payment Risk | 45% | Pre-failure payment alerts, installment options, service continuity |
| Fiber High Price | 37% | Conditional discount, service upgrade, competitive offer matching |

---

## A/B Testing Simulation
- Segments split into **two groups:**
  - **A:** standard offer (control)
  - **B:** new promotion or bonus (test)
- **Churn reduction simulation:**
  - Group B consistently outperforms Group A in **retained customers**, **value saved**, **net value**, and **ROI**
- **Example:**
  - `segment_payment_risk`: 744 retained (B) vs 739 retained (A) → Net Value: $131,460 (B)
  - ROI B >> ROI A, confirming cost-effectiveness
- Recommendation: prioritize Offer B for high-risk segments

---

## Deployment Plan
- **Model Scoring Frequency:** monthly
- **Input:** customer data with features (contract, usage, services)
- **Output:** churn probability score + binary label (at-risk / not at-risk)
- **Retention Trigger:** alerts for high-risk customers to activate targeted campaigns
- **Monitoring:** track AUC, recall, actual churn, and ROI

**Continuous Improvement Loop:**
1. Retrain the model with updated data
2. Adjust thresholds and retention actions
3. Identify new churn drivers and retention levers
4. Align marketing, product, finance, and customer success teams

---

## Conclusion
- **RandomForestClassifier** (aggressive recall strategy) is best for detecting high-risk churners
- Segmentation + A/B testing allows **data-driven prioritization** of retention actions
- High-risk segments should be **actively targeted** with tailored offers
- Stable segments require minimal intervention
- **Expected Impact:** reduction of churn cost, improved retention, optimized resource allocation

---

## References
- Churn modeling best practices: _"Predicting Customer Churn in Telecoms"_  
- Machine learning in retention: RandomForest, LightGBM, XGBoost comparisons
