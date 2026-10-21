# 📊 SaaS Financial & Churn Analytics Dashboard (End-to-End Case Study)

A comprehensive product and financial case study focused on customer churn analysis, risk zone identification, and scenario-based unit economics modeling for a SaaS business.

This project is structured as a **production-grade end-to-end analytical solution**: from raw data hygiene and synthetic feature engineering in Excel (Power Query) to multi-dimensional cohort analysis, financial sensitivity modeling, and building an executive Power BI dashboard with Star Schema modeling, Figma UI container design, and dynamic What-If retention simulations.

**Tools Used:** 
* **Power BI Desktop:** Star Schema Data Modeling, Advanced DAX Engine (Custom Card Indicators, Badges, Dynamic Subtexts), Interactive What-If Scenario Parameters.
* **Excel & Power Query:** Data Cleaning (`null` value handling, type casting), Synthetic Feature Synthesis, Pivot Tables, 2D Sensitivity Matrices (P&L Heatmaps).
* **Figma:** Executive UI/UX Design, Custom Canvas Background Layout & Container Frames.

---

## 🎯 Business Context & Objectives

* **Problem:** The company's unit economics are degrading due to a high baseline Churn Rate (**26.6%**). The business is experiencing an annual revenue leakage with **$139.1K** lost in Monthly Recurring Revenue (MRR).
* **Key Constraint:** Retention budget is allocated exclusively for high-value clients (`Is_High_ARPU`). For all other cohorts, the goal is to identify zero-budget product, onboarding, and UX improvements.

### Tested Business Hypotheses:
1. Premium plans (*Fiber Optic*) trigger severe churn when value perception disconnects from cost without added service layers.
2. Churn peaks in early lifecycle stages (*0-6m Tenure*) due to setup friction and a lack of prompt technical support during onboarding.
3. Manual monthly payment methods (*Month-to-month / Electronic check*) create regular psychological friction, driving passive customer loss.

---

## 🛠️ Solution Architecture & Methodology

```text
Raw Data (.csv) 
  ➔ Excel Power Query (Data Hygiene & Feature Engineering) 
  ➔ Excel Pivot & Slicer Panel (Behavioral Cohort Hunting)
  ➔ Financial Model (2D Sensitivity & Unit Economics Matrices)
  ➔ Power BI Star Schema Model (Dim/Fact Separation & DAX Engine)
  ➔ Figma UI Layout Integration ➔ Executive What-If Dashboard

---

## 🧹 1. Data Cleaning & Feature Engineering (Excel Power Query)

Before modeling, the dataset (`7,032` rows) underwent strict data hygiene and feature synthesis in Power Query:
* **Sanity Checks & Null Handling:** Replaced empty string values in numerical fields with explicit `null` states and cast strict data types (`Double`, `Int64`, `Text`) to eliminate calculation artifacts in downstream aggregations.
* **Feature Engineering:**
  * `Tenure_Group`: Binned continuous monthly tenure into logical lifecycle cohorts (`0-6m`, `6-12m`, `1-2y`, `2+y`).
  * `Lost_Monthly_Revenue`: Dynamically calculated lost revenue dollars resulting specifically from churn events (`Churn = Yes`).
  * `Is_High_ARPU`: Synthetic binary flag isolating high-value VIP accounts based on business constraints.
  * `Is_Price_Issue`: Attribute flag marking price-sensitive churn risk factors.

---

## 📊 2. Exploratory Analysis & Financial Sensitivity Modeling (Excel)

### Multi-Dimensional Cohort Analysis
Using an 8-axis interactive Slicer Panel (`Tenure_Group`, `PaymentMethod`, `OnlineSecurity`, `TechSupport`, `SeniorCitizen`, `PaperlessBilling`, `Is_High_ARPU`, `InternetService`, `DeviceProtection`) linked to cross-filtered Pivot Tables, key baseline metrics were established:
* **Total Churn Volume:** `26.6%` (1,869 churned customers)
* **Monthly Revenue Leakage:** `$139,131 / mo`
* **Total Monthly Revenue Base:** `$455,661 / mo`

### Financial Sensitivity Matrices (P&L Heatmaps)
Directly applying slicer retention deltas into financial forecasts creates an analytical trap (ignoring Cost of Retention and Adoption Rates). To prove economic viability, 2D Sensitivity Matrices were built for each risk cohort, evaluating:
* **Unit Profit (`Profit per unit`)** and **Cumulative Financial Impact (`Cumulative effect ($)`)**.
* Evaluated across varying **Cost of Retention ($3–$10/month)** against realistic **Target Churn rates (30%–75%)**.

---

## 📐 3. Data Modeling & DAX Engine (Power BI)

### Star Schema Architecture
The flat transactional file was restructured into an analytical **Star Schema**:
* **Fact Table:** `Fact_Churn` (Contains IDs, surrogate keys, numerical metrics: `Charges`, `Tenure`, `Lost_Monthly_Revenue`).
* **Dimension Tables:** `Dim_Contract`, `Dim_Demographic`, `Dim_Services`.
* **Measure Repository & Parameters:** Dedicated `_Measures` table and `Target Churn` What-If parameter table.

### Dynamic DAX & UI Formatting Engine
To display clean single-card containers without visual bloat, multi-line composite DAX measures were engineered:
* **Composite KPI Cards:** Measures like `Total Customers Card`, `Lost MRR Card`, `LTV Card`, and `MRR Card` bundle primary metrics with formatted secondary subtexts (e.g., `"5 163 Active + 1 869 Churned"` or `"30.5% of Potential"`).
* **Conditional Badging & Status:** `Churn_Benchmark_Status_Text` and `Churn_Badge_TextColor` dynamically feed alert tags (e.g., `"Above Target (15%)"`).
* **What-If Retention Simulator:** Dynamic slider parameter linking user target thresholds to real-time financial projections (`Saved MRR` & `ARR Impact`).

---

## 🎨 4. Executive UI/UX Design (Figma)

To deliver a polished C-level dashboard, a custom background framework was created in Figma:
* **Structured Canvas Containers:** Dedicated white card containers with soft rounded corners and drop shadows built over a subtle blue gradient background.
* **Design Standards:** Standardized metric icon placement, clear typographic hierarchy, and pre-allocated UI slots for filters, cohort matrices, and simulation widgets.

---

## 🖼️ Dashboard Preview

![Power BI Executive Dashboard](power-bi/screenshots/Dashboard.jpg)

### Executive Dashboard Sections:
1. **Top KPI Header:** High-level executive cards displaying `Total Customers`, `LTV ($230.7)`, `Churn Rate (26.6%)`, `MRR ($316.5K)`, and `Lost MRR ($139.1K)` with embedded trend subtexts.
2. **Customer Churn Detail Panel:** Integrated slicer bar (`InternetService`, `TechSupport`, `OnlineSecurity`, `PaymentMethod`, `Contract`) driving an in-depth cohort matrix broken down by `Tenure`, `Customers`, `ARPU`, `Churn Rate`, `Lifetime`, `LTV`, and `Lost Customers`.
3. **What-If Retention Simulator:** Interactive slider allowing decision-makers to adjust target churn thresholds (e.g., `0.10`) and instantly evaluate bottom-line financial recovery (`Saved MRR: $107.5K` / `ARR Impact: $1.3M`).

---

## 🔀 Key Churn Segments & Strategic Recommendations

1. **High-ARPU Value Failure 💰 *(Requires Retention Budget)*:**
   * **Cohort:** `Is_High_ARPU: High` + `OnlineSecurity: No`
   * **Insight:** Premium customers without security add-ons churn at **53.3%**, creating the largest revenue loss ($64K/mo).
   * **Action:** Assign VIP Account Managers and sponsor *OnlineSecurity* for free during the first 6 months to build ecosystem lock-in.
   * **Model Validation:** Even with $5/mo retention cost at a conservative 35% target churn, cumulative LTV profit equals **+$97,466**.

2. **Core Onboarding Failure 🛑 *(Zero Budget)*:**
   * **Cohort:** `Tenure: 0-6m` + `TechSupport: No` + `Fiber Optic`
   * **Insight:** New clients churn at **53.3%** in early months due to setup frustration, failing to recoup Customer Acquisition Cost (CAC).
   * **Action:** Launch automated trigger-based onboarding emails/SMS on Day 3 of inactivity with video setup guides.

3. **Payment Friction Trap 💳 *(Zero Budget)*:**
   * **Cohort:** `Tenure: 0-6m` + `PaymentMethod: Electronic check`
   * **Insight:** Manual payment methods induce regular billing friction, yielding **73.0%** churn.
   * **Action:** Incentivize auto-pay linkage during registration via gamification (e.g., *"Opt-out auto-pay for a free speed boost"*).

4. **Senior Citizen Tech Gap 👵 *(Zero Budget)*:**
   * **Cohort:** `SeniorCitizen: 1` + `TechSupport: No`
   * **Insight:** Churn hits **53.6%** due to self-service tech support difficulties.
   * **Action:** Route Senior accounts directly to dedicated human support agents, bypassing standard IVR menus.

---

## 🛠️ How to Open & Run the Project Locally

1. Clone or download this repository to your local machine.
2. Open `power-bi/churn_dashboard.pbix` in Power BI Desktop (or `excel/churn_analysis.xlsx` in Excel).
3. Go to **Transform Data ➔ Edit Parameters** (в Power BI / Excel Power Query).
4. Update the `DataFilePath` parameter to match the absolute path of the `/data/` folder on your machine (e.g., `C:/Projects/customer-churn-analysis/data/`).
5. Click **Apply Changes** to refresh all queries and data models.
