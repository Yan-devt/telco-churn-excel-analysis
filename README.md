# 📊 SaaS Financial & Churn Analytics Dashboard

A comprehensive product and financial case study focused on customer churn analysis, risk zone identification, and scenario-based unit economics modeling for a SaaS business.

This project is structured as an end-to-end analytical product: from raw data hygiene and hidden anomaly detection to translating technical metrics into actionable business decisions (with dollar-value impact and retention cost consideration).

**Tools Used:** Excel (Power Query, Pivot Tables, Advanced Data Modeling), Scenario Modeling (Sensitivity Analysis).

---

## 🎯 Business Context & Objectives

**Problem:** The company's unit economics are degrading due to a high Churn Rate. The business is losing money on unrecouped user acquisition.
**Key Constraint:** The retention budget is allocated **exclusively for the most valuable clients** (High-ARPU). For all other segments, the goal is to identify growth areas that do not require direct financial investment.

**Tested Hypotheses:**
1. Premium plans (Fiber Optic) trigger churn after promotional periods end.
2. Churn peaks in the first few months due to a lack of prompt technical support.
3. Manual monthly payments (Month-to-month) create regular psychological friction, increasing churn.

---

## 🛠 Solution Architecture & Methodology

### 1. Data Cleaning & Feature Engineering (Power Query)
Before building the model, raw data underwent rigorous cleaning (sanity checks, duplicate removal, null-value handling). To enable deep cross-analysis, new features were synthesized:
* **Tenure_Group:** Grouping continuous monthly tenure into logical cohorts (0-6m, 6-12m, 1-2y, etc.) to analyze the customer lifecycle.
* **Lost_Monthly_Revenue:** Translating the churn event (Churn = Yes) into actual lost revenue dollars.
* **Is_High_ARPU:** A binary flag to segment VIP clients based on the primary business constraint (retention budget exclusively for High-ARPU).
* **Is_Price_Issue:** A marker indicating price as a probable churn driver.

![Power Query Data Hygiene & Feature Engineering](images/power_query.png)

### 2. Exploratory Data Analysis (EDA) & Slicer Hunting
The company's baseline churn rate was **26.58%**, with a monthly revenue leakage of **$139,131**. 
Using an interactive slicer panel, I ran manual hypothesis testing. By applying cross-filtration, I hunted for narrow cohorts where churn metrics critically exceeded the baseline.

![Interactive Tabular Dashboard and Slicers](images/eda_dashboard.png)

---

## 🔀 Key Churn Segments & Business Solutions

Cross-analysis revealed 4 "epicenters" of financial loss. Tailored solutions were developed for each segment, adhering strictly to budget constraints.

### 1. High-ARPU Value Failure 💰 *Requires Budget*
* **Profile:** `Is_High_ARPU: High` + `OnlineSecurity: No`
* **Problem:** High-ticket clients without additional security add-ons churn at a **53.2%** rate, generating the highest revenue leakage ($64,030 / mo).
* **Solution:** Assign dedicated Account Managers. Invest the budget into providing the `OnlineSecurity` add-on for free during the first 6 months to create an ecosystem lock-in effect.

### 2. Core Onboarding Failure 🛑 *Zero Budget*
* **Profile:** `Tenure: 0-6m` + `TechSupport: No` + `Fiber Optic`
* **Problem:** New clients fail to set up premium internet properly and churn at a **74.9%** rate without support, failing to recoup Customer Acquisition Cost (CAC).
* **Solution:** Implement trigger-based onboarding email chains. If a user is inactive by day 3, initiate an automated push-call or SMS with video instructions.

### 3. Payment Friction Trap 💳 *Zero Budget*
* **Profile:** `Tenure: 0-6m` + `PaymentMethod: Electronic check`
* **Problem:** Manual payment methods create recurring friction—clients simply abandon the service (churn rate: **73.0%**).
* **Solution:** Aggressively transition users to automated payments (Credit Card). Introduce gamification (Opt-out instead of Opt-in during registration): "Link a card today and get a free speed upgrade."

### 4. Senior Citizen Tech Gap 👵 *Zero Budget*
* **Profile:** `SeniorCitizen: 1` + `TechSupport: No` + `Fiber Optic`
* **Problem:** Churn sits at **53.6%**. Elderly clients struggle with complex equipment setup independently.
* **Solution:** Implement a smart call routing algorithm: instantly connect Senior clients to a dedicated "patient" support line, bypassing the standard IVR menu.

---

## 💵 Financial Modeling & Sensitivity Analysis

When filtering the dashboard (e.g., toggling `OnlineSecurity: No` $\rightarrow$ `Yes`), we observe a dramatic churn drop from 53.2% to 22.9%. However, directly multiplying this delta by the revenue base is an **analytical trap** that ignores the real Cost of Retention and Adoption Rate.

To prove economic viability, a **Financial Model (P&L Heatmap)** was developed, factoring in:
1. Cost of Retention per client ($/month).
2. Realistic Target Churn rates.
3. Cannibalization risk (spending budget on clients who would have stayed organically).

![Financial Model and Sensitivity Analysis](images/financial_model.png)

**Model in Action (High-ARPU Segment):**
Instead of the utopian 22.9% churn seen in the ideal slicer cut, the model assumes a conservative `Target Churn` of **35%**. 
Even with these constraints and a retention cost (free security + account manager) of **$5/month** per client, the initiative is profitable, yielding a cumulative LTV profit of **+$97,466**.

---

## 🔄 Analytical Framework: From Insight to Production

This project utilizes a two-step validation system:
1. **Slicers (Behavioral Lab):** Demonstrates the theoretical potential of metric shifts and identifies UX/Product friction points.
2. **Financial Model (Business Filter):** Answers the ultimate question: "Does applying this lever make financial sense?"

### 🚀 Hypothesis Validation Roadmap
Because retrospective data shows correlation, not causality (Selection Bias), the proposed measures are phased for rollout:

* **Quick Wins (Zero-budget UX changes):** The transition to auto-payments is implemented immediately, as it carries no financial risk.
* **Qualitative Research (CustDev):** Conduct exit interviews with 50 churned clients from the `Core Onboarding Failure` group to validate the true root cause (lack of technical support).
* **Causal Validation (A/B Testing):** Solutions requiring budget (e.g., free OnlineSecurity for High-ARPU) are launched as an A/B test on 10% of the cohort. A 100% rollout is only approved upon achieving a statistically significant (p < 0.05) reduction in churn.
