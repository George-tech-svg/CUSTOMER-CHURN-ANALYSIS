#  Telco Customer Churn Analysis – Power BI Dashboard

##  Welcome to My Data Analytics Portfolio Project!

*"Turning raw customer data into million-dollar retention strategies"*

---

##  Project Overview

Imagine you run a phone and internet company with **7,043 customers**. Every month, customers leave to join competitors. That's called **"churn"** – and it's a BIG problem because finding new customers costs **5x more** than keeping existing ones!

**This project answers one critical question:**
> *"Why are customers leaving, and how can we stop it?"*

I built an **interactive 4-page Power BI dashboard** that reveals exactly why customers leave, when they leave, and how much money the company is losing.

---

##  Dashboard Preview

### Page 1: Executive KPI Summary
[Executive KPI Dashboard](EXECUTIVE KPI.png)
| Metric | Value |
|--------|-------|
| Total Customers | 7,043 |
| Churn Rate | 26.54% |
| Monthly Revenue Lost | $139,130 |
| Total Revenue Lost | $2.86 Million |
| Avg Tenure (Churned) | 17.98 months |

### Page 2: Why Churn Happens
![Churn Analysis](image2.png)

**Key insights from this page:**
-  63% of churn happens in first 12 months
-  Customers without Online Security churn 3x more
-  Electronic check users churn 4x more than auto-pay

### Page 3: Contract Impact Matrix
![Contract Analysis](image3.png)

| Contract Type | Churned Customers | Churn Rate |
|--------------|------------------|------------|
| Month-to-month | 1,655 | 42.7%  |
| One year | 166 | 11.3%  |
| Two year | 48 | 2.8%  |

**Month-to-month customers are 15x more likely to churn!**

### Page 4: Action Plan
![Recommendations](image4.png)

Data-backed recommendations to save over **$1.2 Million annually**

---

## Dataset

The **IBM Telco Customer Churn dataset** – 7,043 customers, 21 columns.

| Column | Meaning |
|--------|---------|
| `customerID` | Unique ID per customer |
| `gender` | Male / Female |
| `SeniorCitizen` | 1 = 65+ years old |
| `tenure` | Months as customer (Most important!) |
| `InternetService` | DSL / Fiber optic / No |
| `Contract` | Month-to-month / One year / Two year |
| `PaymentMethod` | Electronic check / Mailed check / Bank transfer / Credit card |
| `MonthlyCharges` | Monthly bill ($18-$120) |
| `TotalCharges` | Lifetime money paid |
| `Churn` | Yes = Left, No = Still a customer |

---

## Key Insights

### Insight #1: The First Year Crisis
**63% of all churn happens in the first 12 months**
> *If a customer survives the first year, they're very likely to stay.*

### Insight #2: Month-to-Month is Dangerous
**Month-to-month customers churn at 15x higher rate than 2-year contracts**

### Insight #3: Electronic Check = Red Flag
**Electronic check users are 4x more likely to churn than auto-pay users**

### Insight #4: Security = Loyalty
**Customers without Online Security churn at 3x higher rate**

---

## Technical Skills Demonstrated

| Skill | What I Did |
|-------|-------------|
| **Power BI** | Built 4-page interactive dashboard with slicers |
| **Power Query** | Cleaned TotalCharges column (null values fixed) |
| **DAX** | Created 9 custom measures including Risk Score |
| **Calculated Columns** | Tenure Group, Senior Status, High Risk Customer |
| **Data Storytelling** | Translated numbers into $1.2M savings |

### DAX Measures Created

```dax
1. Total Customers = COUNTROWS('Table')
2. Churned Customers = CALCULATE(COUNTROWS('Table'), [Churn] = "Yes")
3. Churn Rate % = DIVIDE([Churned Customers], [Total Customers], 0) * 100
4. Retention Rate % = 100 - [Churn Rate %]
5. Monthly Revenue Lost = CALCULATE(SUM([MonthlyCharges]), [Churn] = "Yes")
6. Total Revenue Lost = CALCULATE(SUM([TotalCharges]), [Churn] = "Yes")
7. Avg Monthly Charge (Churned) = CALCULATE(AVERAGE([MonthlyCharges]), [Churn] = "Yes")
8. Avg Tenure (Churned) = CALCULATE(AVERAGE([tenure]), [Churn] = "Yes")
9. Risk Score = AVERAGEX('Table', SWITCH(Contract...) + SWITCH(Payment...) + SWITCH(Security...))

Business Impact
Recommendation	Monthly Savings
Convert 10% of MTM to 1-year contracts	$14,000
Switch 20% of Electronic check to auto-pay	$11,000
Bundle security for fiber customers	$28,000
Improve first-year retention by 10%	$50,000
TOTAL	~$103,000 MONTHLY
That's over $1.2 MILLION annually!

Files in This Repository
File	Description
Telco_Churn_Dashboard.pbix	Power BI file
Dashboard_Export.pdf	PDF of all 4 pages
WA_Fn-UseC_-Telco-Customer-Churn.csv	Raw dataset
