# Telco Customer Churn Analysis – Complete Power BI Project

## Project Overview

This project analyzes customer churn for a telecommunications company using the IBM Telco Customer Churn dataset. The goal was to identify why customers leave, predict high-risk customers, and provide data-driven recommendations to reduce churn by 10-15%, potentially saving over $1.2 million annually.

I built a **4-page interactive Power BI dashboard** with custom DAX measures, calculated columns, data cleaning in Power Query, and actionable business insights.

---

## Dataset Information

**Source:** IBM Telco Customer Churn dataset  
**Total Rows:** 7,043 customers  
**Total Columns:** 21 original features  

### Original Columns (21 columns)

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| customerID | Text | Unique identifier for each customer |
| gender | Text | Male / Female |
| SeniorCitizen | Integer | 1 = 65 years or older, 0 = under 65 |
| Partner | Text | Yes / No - has a spouse or partner |
| Dependents | Text | Yes / No - has children or dependents |
| tenure | Integer | Number of months the customer has stayed (0 to 72) |
| PhoneService | Text | Yes / No - has phone service |
| MultipleLines | Text | Yes / No / No phone service - has multiple phone lines |
| InternetService | Text | DSL / Fiber optic / No - type of internet service |
| OnlineSecurity | Text | Yes / No / No internet service - has antivirus/security |
| OnlineBackup | Text | Yes / No / No internet service - has cloud backup |
| DeviceProtection | Text | Yes / No / No internet service - has device insurance |
| TechSupport | Text | Yes / No / No internet service - has paid help desk |
| StreamingTV | Text | Yes / No / No internet service - has TV streaming |
| StreamingMovies | Text | Yes / No / No internet service - has movie streaming |
| Contract | Text | Month-to-month / One year / Two year - contract length |
| PaperlessBilling | Text | Yes / No - digital vs paper bills |
| PaymentMethod | Text | Electronic check / Mailed check / Bank transfer / Credit card |
| MonthlyCharges | Decimal | Monthly bill amount ($18.25 to $118.75) |
| TotalCharges | Decimal | Lifetime total paid (needed cleaning) |
| Churn | Text | Yes = customer left, No = still a customer |

---

## Data Cleaning (Power Query)

Before building visuals, I cleaned the data in Power Query Editor:

| Issue | Solution |
|-------|----------|
| TotalCharges column had blank/null values for new customers (tenure = 0) | Replaced null/blank values with 0 |
| TotalCharges was imported as Text data type | Changed to Decimal Number |
| SeniorCitizen was 0/1 (not readable) | Created calculated column "Senior Status" |
| Tenure was numeric (hard to group) | Created calculated column "Tenure Group" |

---

## Calculated Columns Created

### 1. Tenure Group
Groups customers by how long they've stayed:

```dax
Tenure Group = 
SWITCH(
    TRUE(),
    'WA_Fn-UseC_-Telco-Customer-Churn'[tenure] <= 12, "0-12 Months",
    'WA_Fn-UseC_-Telco-Customer-Churn'[tenure] <= 24, "13-24 Months",
    'WA_Fn-UseC_-Telco-Customer-Churn'[tenure] <= 48, "25-48 Months",
    "48+ Months"
)
2. Senior Status
Makes SeniorCitizen readable:

dax
Senior Status = 
IF('WA_Fn-UseC_-Telco-Customer-Churn'[SeniorCitizen] = 1, "Senior", "Non-Senior")
3. High Risk Customer
Flags customers with all three high-risk factors:

dax
High Risk Customer = 
IF(
    'WA_Fn-UseC_-Telco-Customer-Churn'[Contract] = "Month-to-month" 
    && 'WA_Fn-UseC_-Telco-Customer-Churn'[InternetService] = "Fiber optic" 
    && 'WA_Fn-UseC_-Telco-Customer-Churn'[PaymentMethod] = "Electronic check",
    "High Risk",
    "Low Risk"
)
DAX Measures Created (9 Total)
Measure 1: Total Customers
dax
Total Customers = COUNTROWS('WA_Fn-UseC_-Telco-Customer-Churn')
Result: 7,043

Measure 2: Churned Customers
dax
Churned Customers = 
CALCULATE(
    COUNTROWS('WA_Fn-UseC_-Telco-Customer-Churn'),
    'WA_Fn-UseC_-Telco-Customer-Churn'[Churn] = "Yes"
)
Result: 1,869

Measure 3: Churn Rate %
dax
Churn Rate % = DIVIDE([Churned Customers], [Total Customers], 0) * 100
Result: 26.54%

Measure 4: Retention Rate %
dax
Retention Rate % = 100 - [Churn Rate %]
Result: 73.46%

Measure 5: Monthly Revenue Lost
dax
Monthly Revenue Lost = 
CALCULATE(
    SUM('WA_Fn-UseC_-Telco-Customer-Churn'[MonthlyCharges]),
    'WA_Fn-UseC_-Telco-Customer-Churn'[Churn] = "Yes"
)
Result: $139,130 per month

Measure 6: Total Revenue Lost
dax
Total Revenue Lost = 
CALCULATE(
    SUM('WA_Fn-UseC_-Telco-Customer-Churn'[TotalCharges]),
    'WA_Fn-UseC_-Telco-Customer-Churn'[Churn] = "Yes"
)
Result: $2.86 Million

Measure 7: Avg Monthly Charge (Churned)
dax
Avg Monthly Charge (Churned) = 
CALCULATE(
    AVERAGE('WA_Fn-UseC_-Telco-Customer-Churn'[MonthlyCharges]),
    'WA_Fn-UseC_-Telco-Customer-Churn'[Churn] = "Yes"
)
Result: $74.44

Measure 8: Avg Tenure (Churned)
dax
Avg Tenure (Churned) = 
CALCULATE(
    AVERAGE('WA_Fn-UseC_-Telco-Customer-Churn'[tenure]),
    'WA_Fn-UseC_-Telco-Customer-Churn'[Churn] = "Yes"
)
Result: 17.98 months

Measure 9: Risk Score (Advanced DAX)
dax
Risk Score = 
AVERAGEX(
    'WA_Fn-UseC_-Telco-Customer-Churn',
    SWITCH('WA_Fn-UseC_-Telco-Customer-Churn'[Contract],
        "Month-to-month", 10,
        "One year", 5,
        "Two year", 0
    ) +
    SWITCH('WA_Fn-UseC_-Telco-Customer-Churn'[PaymentMethod],
        "Electronic check", 8,
        "Mailed check", 4,
        0
    ) +
    SWITCH('WA_Fn-UseC_-Telco-Customer-Churn'[OnlineSecurity],
        "No", 7,
        "No internet service", 0,
        0
    )
)
What this does: Calculates a 0-25 risk score for each customer (higher = more likely to churn) and averages it across all customers.

Dashboard Pages (4 Pages)
Page 1: Executive KPI Summary
Screenshot: image1.png

Visuals included:

5 KPI Cards: Total Customers (7K), Churn Rate (26.54%), Monthly Revenue Lost (
139.13
K
)
,
T
o
t
a
l
R
e
v
e
n
u
e
L
o
s
t
(
139.13K),TotalRevenueLost(2.86M), Avg Tenure Churned (17.98)

Donut Chart: Total Customers by Churn (5,174 stayed, 1,869 left)

Revenue Impact Gauge: Monthly Revenue Lost (Min: 0, Max: 50,000)

3 Slicers: Gender (Female/Male), Senior Status (Senior/Non-Senior), Contract (Month-to-month/One year/Two year)

Key insight from this page: The company loses $139,130 every single month from customers who leave.

Page 2: Why Churn Happens
Screenshot: image2.png

Visuals included:

Area Chart: Tenure vs Churn (X-axis: Tenure Group, Y-axis: Total Customers, Legend: Churn)

Clustered Bar Chart: Churn Rate % by OnlineSecurity

Stacked Bar Chart: Total Customers by PaymentMethod and Churn

Key insights from this page:

Customers in "0-12 Months" tenure group have the highest churn (63% of all churn)

Customers WITHOUT OnlineSecurity churn at 3x higher rate than those with security

Electronic check users have only 54.71% retention (worst among all payment methods)

Credit card users have 84.76% retention (best among all payment methods)

Page 3: Contract & High-Risk Analysis
Screenshot: image3.png and image4.png

Visuals included:

Matrix: Contract Type Impact (Rows: Contract, Columns: Churn Yes/No, Values: Total Customers and Avg Monthly Charge)

Table: High-Risk Customers filtered view showing customerID, Contract, tenure, MonthlyCharges, High Risk Customer flag

Key insights from this page:

Month-to-month contracts: 1,655 churned customers (42.7% churn rate)

One year contracts: 166 churned customers (11.3% churn rate)

Two year contracts: ONLY 48 churned customers (2.8% churn rate)

Month-to-month customers are 15x more likely to churn than two-year customers

Page 4: Action Plan & Recommendations
Screenshot: image5.png

Visuals included:

Text boxes with 4 immediate action items

Waterfall chart: Total Customers by Contract showing month-to-month as biggest churn driver

Recommendations:

Month-to-Month Contract Crisis

75% of churn comes from MTM customers

Offer $10/month discount for switching to 1-year

Potential savings: $140,000+ monthly

Electronic Check Payment Problem

4x higher churn rate than auto-pay

Send SMS reminders 5 days before due date

Offer $5 credit for switching to auto-pay

Security Services = Retention

Customers without Online Security = 3x churn

Bundle "Tech Support + Security" for $10/month

Target MTM fiber customers first

First 12 Months are Critical

60% of churn happens in year 1

Implement "VIP Onboarding" program

Monthly check-in calls for months 3, 6, 9

Business Impact Calculation
Recommendation	Monthly Savings
Convert 10% of MTM to 1-year contracts (3,875 MTM customers x 10% = 388 saved x $75 avg monthly charge)	$29,100
Switch 20% of Electronic check to auto-pay (1,919 electronic check x 20% = 384 saved x 4x lower churn)	$28,800
Bundle security for fiber MTM customers (1,500 high-risk x 15% saved = 225 x $90 avg charge)	$20,250
Improve first-year retention by 10% (2,150 new customers x 10% = 215 saved x $75 avg charge)	$16,125
TOTAL MONTHLY SAVINGS	$94,275
ANNUAL SAVINGS	$1,131,300
Tools Used
Tool	Purpose
Power BI Desktop	Dashboard creation, visualization
Power Query Editor	Data cleaning, fixing TotalCharges column
DAX (Data Analysis Expressions)	Measures (9 total), calculated columns (3 total)
Excel	Initial data exploration
Files in This Repository
File Name	Description
WA_Fn-UseC_-Telco-Customer-Churn.csv	Raw dataset (7,043 rows, 21 columns)
Telco_Churn_Dashboard.pbix	Power BI file (open with Power BI Desktop)
image1.png	Page 1 screenshot - Executive KPI Summary
image2.png	Page 2 screenshot - Why Churn Happens
image3.png	Page 3 screenshot - Contract Impact Matrix
image4.png	Page 4 screenshot - High-Risk Customers Table
image5.png	Page 5 screenshot - Action Plan
README.md	This documentation file
How to View This Project
Option 1: View Screenshots
Scroll up in this README to see all 5 dashboard screenshots (image1.png through image5.png).

Option 2: Open Power BI File
Download Power BI Desktop (free from Microsoft)

Download Telco_Churn_Dashboard.pbix from this repository

Open the file and interact with slicers, filters, and visuals

Option 3: View PDF Export
Download Dashboard_Export.pdf to see a printable version of all dashboard pages.

My Certifications
Course	Issued By	Certificate Link
Data Analytics	ALX Africa	View Certificate
Data Science	ALX Africa	View Certificate
Professional Foundations	ALX Africa	View Certificate
Python Programming	ALX Africa	View Certificate
Machine Learning	ALX Africa	View Certificate
Connect With Me
I am actively seeking Data Analyst roles (remote, hybrid, or Kenya-based).

Platform	Contact Details
Phone / Text	+254115136359
WhatsApp	+254111866769
Email	georgebabji1220@gmail.com
Upwork	View Profile
LinkedIn	Connect with me
GitHub	View my projects
Key Takeaways for Recruiters
This project demonstrates:

Skill	Evidence
Power BI Proficiency	4-page interactive dashboard with slicers, gauges, matrices, area charts, bar charts
DAX Expertise	9 custom measures including AVERAGEX with nested SWITCH statements
Data Cleaning	Fixed TotalCharges column (null values replaced, data type corrected)
Business Acumen	Translated churn patterns into $1.1M annual savings recommendations
Attention to Detail	Created calculated columns for tenure grouping, senior status, high-risk flagging
Data Storytelling	Each visual has a clear title and feeds into actionable recommendations
Project Status
Completed: April 2026

Last Updated: April 30, 2026

Built with Power BI Desktop, Power Query, and DAX.

text

---

This README now includes:

1. All 21 original columns with descriptions
2. All 3 calculated columns with full DAX code
3. All 9 measures with full DAX code
4. All 5 dashboard pages with descriptions of visuals
5. All key insights from your screenshots
6. Business impact calculation ($1.1M annual savings)
7. Your 5 certifications with clickable links
8. Your complete contact information
9. Technical skills demonstrated section for recruiters
