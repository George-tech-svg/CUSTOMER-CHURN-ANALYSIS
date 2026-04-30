# Customer Churn Analysis

## Power BI Dashboard | Telecom Industry | Data Analytics Portfolio

---

## Project Overview

**Goal:** Identify why customers leave a telecom company and how to stop it.

**The Problem:** Acquiring new customers costs 5x more than retaining existing ones. This company is losing 26.5% of its customers annually.

**The Solution:** An interactive Power BI dashboard that reveals churn patterns and provides data-backed retention strategies.

**Result:** Identified $1.2M in potential annual savings through targeted retention efforts.

---

## Key Metrics

| Metric | Value |
|--------|-------|
| Total Customers | 7,043 |
| Churn Rate | 26.54% |
| Monthly Revenue Lost | $139,130 |
| Total Revenue Lost | $2.86 Million |
| Average Tenure of Churned Customers | 17.98 months |

---

## Dashboard Pages

### Page 1: Executive KPI Summary
High-level metrics including total customers, churn rate, and revenue impact.

### Page 2: Churn Driver Analysis
Visualizations showing:
- Churn by tenure group (63% happens in first 12 months)
- Churn by online security status (3x higher without security)
- Churn by payment method (4x higher for electronic check)
- Churn by internet service type

### Page 3: Contract & Customer Analysis
- Matrix showing churn by contract type (Month-to-month: 1,655 churned, Two year: 48 churned)
- Scatter plot of tenure vs monthly charges
- High-risk customer identification

### Page 4: Action Plan
Data-backed recommendations including:
- Convert month-to-month customers with $10/month discount
- Target electronic check payers with SMS reminders
- Bundle security services for fiber customers

---

## Key Insights

**Insight 1: First Year Crisis**
63% of all churn happens in the first 12 months. Customers who stay past one year are highly likely to remain loyal.

**Insight 2: Contract Type Matters**
Month-to-month customers churn at 15x higher rate than two-year contract customers.

| Contract Type | Churned Customers | Churn Rate |
|--------------|------------------|------------|
| Month-to-month | 1,655 | 42.7% |
| One year | 166 | 11.3% |
| Two year | 48 | 2.8% |

**Insight 3: Payment Method Risk**
Electronic check users are 4x more likely to churn than auto-pay users. Manual payments lead to forgotten bills, service cutoff, and frustration.

**Insight 4: Security Drives Loyalty**
Customers without online security churn at 3x higher rate. Security services are retention tools, not just add-ons.

---

## Technical Skills Demonstrated

| Skill | Application |
|-------|-------------|
| Power BI | Built 4-page interactive dashboard with slicers and page navigation |
| Power Query | Cleaned TotalCharges column (handled null values for new customers) |
| DAX | Created 9 custom measures including Risk Score using AVERAGEX and SWITCH |
| Calculated Columns | Created Tenure Group, Senior Status, High Risk Customer |
| Data Storytelling | Translated analysis into $1.2M actionable recommendations |

---

## DAX Measures Created
Total Customers = COUNTROWS('Table')

Churned Customers = CALCULATE(COUNTROWS('Table'), [Churn] = "Yes")

Churn Rate % = DIVIDE([Churned Customers], [Total Customers], 0) * 100

Retention Rate % = 100 - [Churn Rate %]

Monthly Revenue Lost = CALCULATE(SUM([MonthlyCharges]), [Churn] = "Yes")

Total Revenue Lost = CALCULATE(SUM([TotalCharges]), [Churn] = "Yes")

Avg Monthly Charge (Churned) = CALCULATE(AVERAGE([MonthlyCharges]), [Churn] = "Yes")

Avg Tenure (Churned) = CALCULATE(AVERAGE([tenure]), [Churn] = "Yes")

Risk Score = AVERAGEX('Table',
SWITCH([Contract], "Month-to-month", 10, "One year", 5, "Two year", 0) +
SWITCH([PaymentMethod], "Electronic check", 8, "Mailed check", 4, 0) +
SWITCH([OnlineSecurity], "No", 7, 0))

text

---

## Business Impact

| Recommendation | Monthly Savings |
|----------------|-----------------|
| Convert 10% of month-to-month to 1-year contracts | $14,000 |
| Switch 20% of electronic check to auto-pay | $11,000 |
| Bundle security for fiber customers | $28,000 |
| Improve first-year retention by 10% | $50,000 |
| **Total Potential Savings** | **$103,000 per month** |

**Annual impact: $1,236,000**

---

## Dataset

**Source:** IBM Telco Customer Churn dataset

**Size:** 7,043 customers, 21 columns

| Column | Description |
|--------|-------------|
| customerID | Unique identifier per customer |
| gender | Male / Female |
| SeniorCitizen | 1 = 65+ years old |
| tenure | Months as customer (most important predictor) |
| InternetService | DSL / Fiber optic / No internet |
| Contract | Month-to-month / One year / Two year |
| PaymentMethod | Electronic check / Mailed check / Bank transfer / Credit card |
| MonthlyCharges | Monthly bill amount ($18 - $120) |
| TotalCharges | Lifetime total paid |
| Churn | Yes = left company / No = still a customer |

---

## Files in This Repository

| File | Description |
|------|-------------|
| Customer_Churn_Analysis.pbix | Power BI dashboard file |
| Dashboard_Export.pdf | PDF export of all 4 pages |
| WA_Fn-UseC_-Telco-Customer-Churn.csv | Raw dataset |

---

## Certifications

| Course | Certificate Link |
|--------|------------------|
| Data Analytics | [View Certificate](https://savanna.alxafrica.com/certificates/T95s3SPMxZ) |
| Data Science | [View Certificate](https://savanna.alxafrica.com/certificates/flJSZ2Xs6r) |
| Professional Foundations | [View Certificate](https://savanna.alxafrica.com/certificates/RYz9rB28SJ) |
| Python Programming | [View Certificate](https://savanna.alxafrica.com/certificates/Ee8x6JfGCh) |
| Machine Learning | [View Certificate](https://savanna.alxafrica.com/certificates/7zsMrEN5m2) |

---

## Contact

I am actively seeking Data Analyst roles. Remote, hybrid, or Kenya-based opportunities welcome.

| Method | Contact |
|--------|---------|
| Phone / Text | +254115136359 |
| WhatsApp | +254111866769 |
| Email | georgebabji1220@gmail.com |
| Upwork | [View Profile](https://upwork.com/freelancers/~01ea729f5447c9e73b) |
| LinkedIn | [Connect with me](https://www.linkedin.com/in/george-onyango-5a5906360/) |
| GitHub | [View my projects](https://github.com/George-tech-svg) |

---

## How to View This Project

**Option 1:** Open Dashboard_Export.pdf to see all 4 dashboard pages

**Option 2:** Download Power BI Desktop (free) and open Customer_Churn_Analysis.pbix to interact with the dashboard

**Option 3:** View screenshots above in this README

---

## Tools Used

- Power BI Desktop
- Power Query
- DAX (Data Analysis Expressions)
- Excel

---
