# Customer Churn Analysis

## Power BI Dashboard | Telecom Industry | Data Analytics Portfolio

---

## Project Overview

**Goal:** Identify why customers leave a telecom company and how to stop it.

**The Problem:** Acquiring new customers costs 5x more than retaining existing ones. This analysis focuses on month-to-month contract customers who show a 42.71% churn rate.

**The Solution:** An interactive Power BI dashboard that reveals churn patterns, highlights high-risk customers, and provides data-backed retention strategies.

**Result:** Identified over $1.45M in annual revenue at risk with clear actions to reduce churn.

---

## Key Metrics (Filtered: Month-to-Month Contract)

| Metric | Value |
|--------|-------|
| Total Customers (Month-to-Month) | 3,875 |
| Churn Rate (Month-to-Month) | 42.71% |
| Monthly Revenue Lost | $120,850 |
| Total Revenue Lost | $1.93 Million |
| Average Tenure of Churned Customers | 14.02 months |

---

## Dashboard Pages

### Page 1: Executive KPI Summary (Month-to-Month Focus)

High-level metrics showing the severe impact of month-to-month contracts:
- 3,875 customers on month-to-month contracts
- 42.71% churn rate among these customers
- $120,850 monthly revenue lost
- $1.93 million total lifetime revenue lost
- Average churned customer stayed only 14 months

Filters available: Gender, Senior Status, Contract Type

### Page 2: Churn Driver Analysis

Visualizations showing:
- Tenure vs Churn: Highest churn occurs in first 12 months
- Churn by Contract Type: Month-to-month customers drive majority of churn
- Churn by Payment Method: Electronic check users show highest churn rates
- Churn by Internet Service: Fiber optic customers churn more than DSL

### Page 3: Contract Impact Matrix

| Contract Type | Total Customers | Churned Customers | Churn Rate |
|--------------|----------------|------------------|------------|
| Month-to-month | 3,875 | 1,655 | 42.71% |
| One year | 1,473 | 166 | 11.27% |
| Two year | 1,695 | 48 | 2.83% |

**Key finding:** Month-to-month customers are 15 times more likely to churn than two-year contract customers.

### Page 4: High-Risk Customer Identification

The dashboard flags individual high-risk customers based on:
- Contract type (Month-to-month = High Risk)
- Payment method (Electronic check = High Risk)
- Security services (No online security = High Risk)

Sample high-risk customers identified:
- 0129-KPTWJ: Month-to-month contract, 72 months tenure, $94.65 monthly charge

### Page 5: Demographic Analysis

Churn rate breakdown by:
- Gender: Female vs Male churn patterns
- Senior Status: Senior vs Non-Senior churn rates
- Combination views showing churn by gender and senior status simultaneously

---

## Key Insights

**Insight 1: Month-to-Month Contract Crisis**
42.71% of month-to-month customers churn compared to only 2.83% of two-year contract customers. This is the single biggest churn driver.

**Insight 2: First Year is Critical**
Most churn occurs within the first 12 months of tenure. Customers who stay beyond one year are significantly more likely to remain loyal.

**Insight 3: Revenue at Risk**
Month-to-month customers alone represent $1.93 million in total revenue lost and $120,850 in monthly recurring revenue lost.

**Insight 4: High-Risk Customers Can be Identified**
The dashboard flags individual high-risk customers using contract type, payment method, and security service data.

**Insight 5: Demographics Matter**
Churn rates vary significantly by gender and senior status, allowing for targeted retention campaigns.

---

## Technical Skills Demonstrated

| Skill | Application |
|-------|-------------|
| Power BI | Built multi-page interactive dashboard with slicers and filters |
| Power Query | Cleaned TotalCharges column and handled data type issues |
| DAX | Created custom measures including Churn Rate, Revenue Lost, Risk Score |
| Calculated Columns | Created Tenure Group, Senior Status, High Risk Customer flag |
| Data Storytelling | Translated analysis into actionable business recommendations |

---

## DAX Measures Created

```dax
// Core Measures
Total Customers = COUNTROWS('Table')

Churned Customers = CALCULATE(COUNTROWS('Table'), [Churn] = "Yes")

Churn Rate % = DIVIDE([Churned Customers], [Total Customers], 0) * 100

Monthly Revenue Lost = CALCULATE(SUM([MonthlyCharges]), [Churn] = "Yes")

Total Revenue Lost = CALCULATE(SUM([TotalCharges]), [Churn] = "Yes")

Avg Tenure (Churned) = CALCULATE(AVERAGE([tenure]), [Churn] = "Yes")

// Risk Score Measure
Risk Score = 
AVERAGEX('Table',
    SWITCH([Contract], "Month-to-month", 10, "One year", 5, "Two year", 0) +
    SWITCH([PaymentMethod], "Electronic check", 8, "Mailed check", 4, 0) +
    SWITCH([OnlineSecurity], "No", 7, 0)
)

// Calculated Column for High Risk Flag
High Risk Customer = 
IF([Contract] = "Month-to-month" && [PaymentMethod] = "Electronic check", "High Risk", "Low Risk")
