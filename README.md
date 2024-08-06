# BANK LOAN REPORT

## DASHBOARD 1: SUMMARY

- **Click on the Picture to see the Dynamic Dashboard**

[![Bank Loan Report- Summary ](https://github.com/user-attachments/assets/aafbb813-6e87-4113-8649-2c0c0df1d3d5)](https://public.tableau.com/app/profile/ahmed.nassar3033/viz/BankLoanReport_17229751035420/DETAILS?publish=yes)



"In order to monitor and assess our bank's lending activities and performance, we need to create a comprehensive Bank Loan Report. This report aims to provide insights into key loan-related metrics and their changes over time. The report will help us make data-driven decisions, track our loan portfolio's health, and identify trends that can inform our lending strategies.

## 1. Key Performance Indicators (KPIs) Requirements:

1. **Total Loan Applications:** We need to calculate the total number of loan applications received during a specified period. Additionally, it is essential to monitor the Month-to-Date (MTD) Loan Applications and track changes Month-over-Month (MoM).

``` sql
-- Total Loan Applications
SELECT COUNT(id) AS Total_Applications FROM financial_loan;

-- MTD Loan Applications
SELECT COUNT(id) AS MTD_Total_Applications FROM financial_loan
WHERE MONTH(issue_date) = 12 and YEAR(issue_date) = 2021;

-- Previous Month to date  (PMTD) Loan Applications
SELECT COUNT(id) AS PMTD_Total_Applications FROM financial_loan
WHERE MONTH(issue_date) = 11 and year(issue_date) =2021;

-- To find MoM apply this formula [( MTD - PMTD)/PMTD]
```


2. **Total Funded Amount :** Understanding the total amount of funds disbursed as loans is crucial. We also want to keep an eye on the MTD Total Funded Amount and analyse the Month-over-Month (MoM) changes in this metric.

```sql
-- Total Funding Amount 
SELECT SUM(loan_amount) AS MTD_Total_Funded_Amount FROM financial_loan;

-- MTD Total Funding Amount 
SELECT SUM(loan_amount) AS PMTD_Total_Funded_Amount FROM financial_loan
WHERE MONTH(issue_date) = 12 and year(issue_date) =2021;

-- PMTD Total Funding Amount 
SELECT SUM(loan_amount) AS PMTD_Total_Funded_Amount FROM financial_loan
WHERE MONTH(issue_date) = 11 and year(issue_date) =2021;

-- To find MOM apply this formula [( MTD - PMTD)/PMTD]
```


3. **Total Amount Received :** Tracking the total amount received from borrowers is essential for assessing the bank's cash flow and loan repayment. We should analyse the Month-to-Date (MTD) Total Amount Received and observe the Month-over-Month (MoM) changes.

``` sql
-- Total Amount Received
SELECT SUM(total_payment) AS Total_Amount_Collected FROM financial_loan;

-- MTD Total Amount Received
SELECT SUM(total_payment) AS MTD_Total_Amount_Collected FROM financial_loan
WHERE MONTH(issue_date) = 12 and year(issue_date)=2021;

-- PMTD Total Amount Received
SELECT SUM(total_payment) AS PMTD_Total_Amount_Collected FROM financial_loan
WHERE MONTH(issue_date) = 11 and year(issue_date)=2021;
```

4. **Average Interest Rate:** Calculating the average interest rate across all loans, MTD, and monitoring the Month-over-Month (MoM) variations in interest rates will provide insights into our lending portfolio's overall cost.

``` sql
-- Average Interest Rate%
SELECT ROUND(AVG(int_rate)*100 ,4) AS Avg_Int_Rate FROM financial_loan;

-- MTD Average Interest
SELECT ROUND(AVG(int_rate)*100,4) AS MTD_Avg_Int_Rate FROM financial_loan
WHERE MONTH(issue_date) = 12 and year(issue_date)=2021;

-- PMTD Average Interest
SELECT ROUND(AVG(int_rate)*100 ,4) AS PMTD_Avg_Int_Rate FROM financial_loan
WHERE MONTH(issue_date) = 11 and year(issue_date)=2021;

-- To find MOM apply this formula [( MTD - PMTD)/PMTD]
```

5. **Average Debt-to-Income Ratio (DTI)**: Evaluating the average DTI for our borrowers helps us gauge their financial health. We need to compute the average DTI for all loans, MTD, and track Month-over-Month (MoM) fluctuations.

```
-- Avg Debt to Income Ratio(DTI)
SELECT ROUND(AVG(dti)*100,4)  AS Avg_DTI FROM financial_loan;

-- MTD Avg DTI
SELECT ROUND(AVG(dti)*100,4) AS MTD_Avg_DTI FROM financial_loan
WHERE MONTH(issue_date) = 12 and year(issue_date)=2021;

-- PMTD Avg DTI
SELECT ROUND(AVG(dti)*100,4) AS PMTD_Avg_DTI FROM financial_loan
WHERE MONTH(issue_date) = 11 and year(issue_date)=2021;

-- To find MOM apply this formula [( MTD - PMTD)/PMTD]
```

## 2. Good Loan v Bad Loan KPI’s

In order to evaluate the performance of our lending activities and assess the quality of our loan portfolio, we need to create a comprehensive report that distinguishes between 'Good Loans' and 'Bad Loans' based on specific loan status criteria

-  **Good Loan KPIs:**

  1. **Good Loan Application Percentage:** We need to calculate the percentage of loan applications classified as 'Good Loans.' This category includes loans with a loan status of 'Fully Paid' and 'Current.'

```sql
-- Good Loan Applications Percentage
SELECT
    (COUNT(CASE WHEN loan_status = 'Fully Paid' OR loan_status = 'Current' THEN id END) * 100.0) / 
	COUNT(id) AS Good_Loan_Percentage
FROM financial_loan;
```

 2. **Good Loan Applications:** Identifying the total number of loan applications falling under the 'Good Loan' category, which consists of loans with a loan status of 'Fully Paid' and 'Current.'

```sql
-- Good Loan Applications
SELECT COUNT(id) AS Good_Loan_Applications FROM financial_loan
WHERE loan_status = 'Fully Paid' OR loan_status = 'Current';
```
 3. **Good Loan Funded Amount**: Determining the total amount of funds disbursed as 'Good Loans.' This includes the principal amounts of loans with a loan status of 'Fully Paid' and 'Current.'

```sql
    -- Good Loan Funded Amount
SELECT SUM(loan_amount) AS Good_Loan_Funded_amount FROM financial_loan
WHERE loan_status = 'Fully Paid' OR loan_status = 'Current';
```
 4. **Good Loan Total Received Amount:** Tracking the total amount received from borrowers for 'Good Loans,' which encompasses all payments made on loans with a loan status of 'Fully Paid' and 'Current.'

```sql
-- Good Loan Amount Received
SELECT SUM(total_payment) AS Good_Loan_amount_received FROM financial_loan
WHERE loan_status = 'Fully Paid' OR loan_status = 'Current';
```
- **Bad Loan KPIs:**

1. **Bad Loan Application Percentage:** Calculating the percentage of loan applications categorized as 'Bad Loans.' This category specifically includes loans with a loan status of 'Charged Off.'
```sql
-- Bad Loan  Applications Percentage
SELECT
    (COUNT(CASE WHEN loan_status = 'Charged Off' THEN id END) * 100.0) / 
	COUNT(id) AS Bad_Loan_Percentage
FROM financial_loan;
```
  2.  **Bad Loan Applications:** Identifying the total number of loan applications categorized as 'Bad Loans,' which consists of loans with a loan status of 'Charged Off.'

```sql
-- Bad Loan Applications
SELECT COUNT(id) AS Bad_Loan_Applications FROM financial_loan
WHERE loan_status = 'Charged Off';
```
  3.  **Bad Loan Funded Amount:** Determining the total amount of funds disbursed as 'Bad Loans.' This comprises the principal amounts of loans with a loan status of 'Charged Off.'

```sql
-- Bad Loan Funded Amount
SELECT SUM(loan_amount) AS Bad_Loan_Funded_amount FROM financial_loan
WHERE loan_status = 'Charged Off';
```
  4. **Bad Loan Total Received Amount:** Tracking the total amount received from borrowers for 'Bad Loans,' which includes all payments made on loans with a loan status of 'Charged Off.'

```sql
-- Bad Loan Amount Received
SELECT SUM(total_payment) AS Bad_Loan_amount_received FROM financial_loan
WHERE loan_status = 'Charged Off';
```

## Loan Status Grid View

In order to gain a comprehensive overview of our lending operations and monitor the performance of loans, we aim to create a grid view report categorized by 'Loan Status.' This report will serve as a valuable tool for analysing and understanding the key indicators associated with different loan statuses. By providing insights into metrics such as 'Total Loan Applications,' 'Total Funded Amount,' 'Total Amount Received,' 'Month-to-Date (MTD) Funded Amount,' 'MTD Amount Received,' 'Average Interest Rate,' and 'Average Debt-to-Income Ratio (DTI),' this grid view will empower us to make data-driven decisions and assess the health of our loan portfolio.

- **Loan Status:**

```sql
-- LOAN STATUS
	SELECT
        loan_status,
        COUNT(id) AS LoanCount,
        SUM(total_payment) AS Total_Amount_Received,
        SUM(loan_amount) AS Total_Funded_Amount,
        AVG(int_rate * 100) AS Interest_Rate,
        AVG(dti * 100) AS DTI
    FROM
        financial_loan
    GROUP BY
        loan_status;
```

**- Loan Statment:** 

```sql
-- MTD LOAN STATMENT 
SELECT 
	loan_status, 
	SUM(total_payment) AS MTD_Total_Amount_Received, 
	SUM(loan_amount) AS MTD_Total_Funded_Amount 
FROM financial_loan
WHERE MONTH(issue_date) = 12 
GROUP BY loan_status;
```

# DASHBOARD 2: OVERVIEW

- **Click on the Picture to see the Dynamic Dashboard**

[![Bank Loan Report- Overview ](https://github.com/user-attachments/assets/fcee775d-960f-4954-9d9c-20da0d00d1bd)](https://public.tableau.com/app/profile/ahmed.nassar3033/viz/BankLoanReport_17229751035420/DETAILS?publish=yes)



In our Bank Loan Report project, we aim to visually represent critical loan-related metrics and trends using a variety of chart types. These charts will provide a clear and insightful view of our lending operations, facilitating data-driven decision-making and enabling us to gain valuable insights into various loan parameters. Below are the specific chart requirements:


### Monthly Trends by Issue Date (Line Chart) :

- **Chart Type:** Line Chart
- **Metrics:** 'Total Loan Applications,' 'Total Funded Amount,' and 'Total Amount Received'
- **X-Axis:** Month (based on 'Issue Date')
- **Y-Axis:** Metrics' Values
- **Objective:** This line chart will showcase how 'Total Loan Applications,' 'Total Funded Amount,' and 'Total Amount Received' vary over time, allowing us to identify seasonality and long-term trends in lending activities.

```sql
-- MONTH
SELECT 
	MONTH(issue_date) AS Month_Munber, 
	MONTHNAME(issue_date) AS Month_name, 
	COUNT(id) AS Total_Loan_Applications,
	SUM(loan_amount) AS Total_Funded_Amount,
	SUM(total_payment) AS Total_Amount_Received
FROM financial_loan
GROUP BY MONTH(issue_date), MONTHNAME(issue_date)
ORDER BY MONTH(issue_date);
```
### Regional Analysis by State (Filled Map):

- **Chart Type:** Filled Map
- **Metrics:** 'Total Loan Applications,' 'Total Funded Amount,' and 'Total Amount Received'
- **Geographic Regions:** States
- **Objective:** This filled map will visually represent lending metrics categorized by state, enabling us to identify regions with significant lending activity and assess regional disparities.

```sql
-- STATE
SELECT 
	address_state AS State, 
	COUNT(id) AS Total_Loan_Applications,
	SUM(loan_amount) AS Total_Funded_Amount,
	SUM(total_payment) AS Total_Amount_Received
FROM financial_loan
GROUP BY address_state
ORDER BY address_state;
```

### Loan Term Analysis (Donut Chart):

- **Chart Type:** Donut Chart
- **Metrics:** 'Total Loan Applications,' 'Total Funded Amount,' and 'Total Amount Received'
- **Segments:** Loan Terms (e.g., 36 months, 60 months)
- **Objective:** This donut chart will depict loan statistics based on different loan terms, allowing us to understand the distribution of loans across various term lengths.

```sql
-- TERM
SELECT 
	term AS Term, 
	COUNT(id) AS Total_Loan_Applications,
	SUM(loan_amount) AS Total_Funded_Amount,
	SUM(total_payment) AS Total_Amount_Received
FROM financial_loan
GROUP BY term
ORDER BY term;
```

### Employee Length Analysis (Bar Chart):

- **Chart Type:** Bar Chart
- **Metrics:** 'Total Loan Applications,' 'Total Funded Amount,' and 'Total Amount Received'
- **X-Axis:** Employee Length Categories (e.g., 1 year, 5 years, 10+ years)
- **Y-Axis:** Metrics' Values
- **Objective:** This bar chart will illustrate how lending metrics are distributed among borrowers with different employment lengths, helping us assess the impact of employment history on loan applications.

```sql
-- EMPLOYEE LENGTH
SELECT 
	emp_length AS Employee_Length, 
	COUNT(id) AS Total_Loan_Applications,
	SUM(loan_amount) AS Total_Funded_Amount,
	SUM(total_payment) AS Total_Amount_Received
FROM financial_loan
GROUP BY emp_length
ORDER BY emp_length;
```

### Loan Purpose Breakdown (Bar Chart):

- **Chart Type:** Bar Chart
- **Metrics:** 'Total Loan Applications,' 'Total Funded Amount,' and 'Total Amount Received'
- **X-Axis:** Loan Purpose Categories (e.g., debt consolidation, credit card refinancing)
- **Y-Axis:** Metrics' Values
- **Objective:** This bar chart will provide a visual breakdown of loan metrics based on the stated purposes of loans, aiding in the understanding of the primary reasons borrowers seek financing.

```sql
-- PURPOSE
SELECT 
	purpose AS PURPOSE, 
	COUNT(id) AS Total_Loan_Applications,
	SUM(loan_amount) AS Total_Funded_Amount,
	SUM(total_payment) AS Total_Amount_Received
FROM financial_loan
GROUP BY purpose
ORDER BY purpose;
```

### Home Ownership Analysis (Tree Map):

- **Chart Type:** Tree Map
- **Metrics:** 'Total Loan Applications,' 'Total Funded Amount,' and 'Total Amount Received'
- **Hierarchy:** Home Ownership Categories (e.g., own, rent, mortgage)
- **Objective:** This tree map will display loan metrics categorized by different home ownership statuses, allowing for a hierarchical view of how home ownership impacts loan applications and disbursements.

```sql
-- HOME OWNERSHIP
SELECT 
	home_ownership AS Home_Ownership, 
	COUNT(id) AS Total_Loan_Applications,
	SUM(loan_amount) AS Total_Funded_Amount,
	SUM(total_payment) AS Total_Amount_Received
FROM financial_loan
GROUP BY home_ownership
ORDER BY home_ownership;
```

# DASHBOARD 3: DETAILS

- **Click on the Picture to see the Dynamic Dashboard**

[![Bank Loan Report- Details](https://github.com/user-attachments/assets/f9c405b5-ad4e-42db-b18f-923d0b7001b2)](https://public.tableau.com/app/profile/ahmed.nassar3033/viz/BankLoanReport_17229751035420/DETAILS?publish=yes)


In our Bank Loan Report project, we recognize the need for a comprehensive 'Details Dashboard' that provides a consolidated view of all the essential information within our loan data. This Details Dashboard aims to offer a holistic snapshot of key loan-related metrics and data points, enabling users to access critical information efficiently.

**Objective:**
The primary objective of the Details Dashboard is to provide a comprehensive and user-friendly interface for accessing vital loan data. It will serve as a one-stop solution for users seeking detailed insights into our loan portfolio, borrower profiles, and loan performance.












