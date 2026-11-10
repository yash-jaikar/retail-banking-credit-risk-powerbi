📌 Retail Banking Loan Portfolio & Credit Risk Analysis (Power BI)
🎯 Executive Summary & Business Problem
Why does this matter?

In retail banking, maintaining a balanced credit portfolio and mitigating default risk is far more cost-effective than managing bad debt recovery or acquiring replacement capital. A poorly calibrated underwriting strategy leads to either excessive credit risk or lost revenue from overly stringent rejections.

This project analyzes an executive dataset of 50,031 loan applications to uncover key credit risk drivers, evaluate applicant financial health, and identify optimal underwriting parameters that balance portfolio growth with risk containment.

📊 Key Business Insights (The "So What?")
Credit Score Risk Boundary: Applicants categorized in the Poor credit tier exhibit significantly lower approval rates, but intermediate tiers (Fair / Good) represent a high-potential segment currently constrained by rigid approval cutoffs.

Debt Stress Sensitivity: Applicants with a High Debt-to-Income (DTI > 40%) ratio account for the highest concentration of high-risk exposures, underscoring DTI as a primary gatekeeper for portfolio default prevention.

Income & Employment Stability: Salaried applicants with 5+ years of employment tenure display remarkably lower risk profiles regardless of loan size, indicating that employment stability serves as a strong counterweight to moderate credit scores.

Capital Exposure Distribution: A substantial portion of total disbursed capital is concentrated in mid-to-high loan amount requests, highlighting the need for dynamic interest rate pricing rather than flat approval/rejection decisions.

🛠️ Project Scope & Dataset Overview
Total Application Records: 50,031 entries

Demographic Metrics: Age, Gender, Income Brackets, Marital Status, Dependents, Location/State.

Employment Metrics: Employment Status (Salaried, Self-Employed, Contractual), Tenure at Current Job.

Credit & Debt Metrics: Credit Score, Debt-to-Income Ratio (DTI), Outstanding Debts, Prior Defaults.

Financial Exposure: Loan Amount Requested, Term Duration, Interest Rates, Disbursed Capital, Approval Status (Approved vs. Rejected).

🏗️ Technical Architecture & DAX Implementation
1. Custom DAX Categorizations (Calculated Columns)
To enable granular filtering and executive segmentation across report views, custom DAX logic was built:

Credit Score Tiering:

Code snippet
Credit_Tier = 
SWITCH(
    TRUE(),
    [Credit_Score] >= 750, "Excellent",
    [Credit_Score] >= 700, "Good",
    [Credit_Score] >= 650, "Fair",
    "Poor"
)
Debt-to-Income (DTI) Risk Categories:

Code snippet
DTI_Category = 
SWITCH(
    TRUE(),
    [DTI] <= 0.20, "Low Risk (<20%)",
    [DTI] <= 0.40, "Moderate Risk (20-40%)",
    "High Risk (>40%)"
)
Income Brackets:

Code snippet
Income_Bracket = 
SWITCH(
    TRUE(),
    [Income] < 30000, "Low Income (<$30K)",
    [Income] <= 80000, "Middle Income ($30K-$80K)",
    "High Income (>$80K)"
)
2. Core Measures & Aggregations
Application Counts:

Total_Applications = COUNT('Bank dataset 2'[Application_ID])

Approved_Loans = CALCULATE([Total_Applications], 'Bank dataset 2'[Status] = "Approved")

Rejected_Loans = CALCULATE([Total_Applications], 'Bank dataset 2'[Status] = "Rejected")

Performance Ratios:

Approval_Rate = DIVIDE([Approved_Loans], [Total_Applications], 0)

Rejection_Rate = DIVIDE([Rejected_Loans], [Total_Applications], 0)

Portfolio Risk Averages:

Avg_Credit_Score = AVERAGE('Bank dataset 2'[Credit_Score])

Avg_DTI = AVERAGE('Bank dataset 2'[DTI])

Total_Disbursed_Capital = CALCULATE(SUM('Bank dataset 2'[Loan_Amount]), 'Bank dataset 2'[Status] = "Approved")

💡 Strategic Recommendations
Dynamic Risk-Based Pricing: Shift away from binary approval/rejection for "Fair" credit tiers by introducing tiered interest rate margins to capture loan volume safely.

Contextual DTI Thresholds: Adjust DTI limits according to employment stability and income tier, allowing stable salaried earners with slightly elevated DTI ratios to qualify.

Automated Approval Workflows: Fast-track approvals for applicants in the Excellent Credit Tier with Low DTI to increase operational efficiency and customer conversion.

Targeted Risk Audits: Conduct routine stress tests on disbursements within the Moderate to High DTI buckets to prevent portfolio degradation.

💻 Tech Stack & Visual Design
Business Intelligence Tool: Power BI Desktop

Data Transformation & Modeling: Power Query (ETL), DAX (Data Analysis Expressions)

UI & Theme: Executive color palette featuring Hex #666666 accents, clean KPI cards, and cross-filterable visual layouts.
