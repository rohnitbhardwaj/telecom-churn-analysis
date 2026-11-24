# Telecom Customer Churn Analysis

**Excel · Python · Power BI**

📊 [View Dashboard Screenshots](https://github.com/rohnitbhardwaj/telecom-churn-analysis/blob/main/dashboard)

## Business Problem

Which customers are most likely to leave, and which segments account for the greatest loss of monthly revenue? The goal is to help a telecom retention team identify higher-risk customer segments and decide where to focus retention offers.

## Dataset

Telco Customer Churn data, a sample dataset from a fictional telecom company. It covers 7,043 customers, including 1,869 who churned (26.5%).

[Dataset source -- Kaggle](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)

## Methodology

1. **Data preparation (Excel Power Query):** Changed customer ID to text, converted `TotalCharges` to a number (11 blanks set to 0, all customers with zero tenure), recoded `SeniorCitizen` to Yes/No, standardised "No phone/internet service" labels to "No", shortened payment method names and added a tenure group column (0-12, 13-24, 25-48, 49+ months). Every step is documented in the [cleaning log](excel/data_cleaning_log.md).
2. **Exploration (Excel):** Pivot tables to check churn rates by tenure group, contract, internet service, payment method, paperless billing and tech support.
3. **Analysis (Python):** Reproduced the Excel results in pandas, calculated revenue lost to churn, compared churned and retained customers, defined a high-risk segment and tested it with 95% confidence intervals (Wilson method), a chi-square test and Cramér's V, and built seaborn charts and a correlation heatmap.
4. **Visualization (Power BI):** A 2-page dashboard with DAX measures and slicers: overall churn drivers, and the high-risk segment with the revenue it costs.

## Key Findings

* **Tenure:** Churn falls from 47.4% in the first 12 months to 9.5% for customers with 49+ months. About half of the monthly revenue lost to churn comes from first-year customers.
* **Contract:** Month-to-month customers churn at 42.7%, versus 11.3% for one-year and 2.8% for two-year contracts. Month-to-month contracts account for about 87% of lost revenue.
* **Internet service:** Fiber optic customers churn at 41.9%, versus 19.0% for DSL and 7.4% for customers without internet. Fiber customers also pay more (about 91.5 vs 58 a month), but among internet customers the median bill is similar for churned and retained customers, so price alone does not explain churn.
* **Payment method:** Electronic check customers churn at 45.3%, versus 15.2% to 19.1% for other methods.
* **Tech support:** Among internet customers, those without tech support churn at 41.6%, versus 15.2% for those with it.
* **High-risk segment:** Month-to-month customers on fiber optic who pay by electronic check (1,307 customers, 18.6% of the base) churn at 60.4% (95% CI: 57.7% to 63.0%), versus 18.8% (17.8% to 19.9%) for everyone else, about 3.2 times the rate, and account for about 49% of lost revenue. The difference is statistically significant (chi-square p < 0.001, Cramér's V = 0.37).

## Business Impact

* Churned customers are 26.5% of the base but 30.5% of monthly revenue, because they tend to be higher-paying customers. Approximately $139K in monthly recurring revenue is associated with customers who have churned.
* The high-risk segment is under a fifth of customers but about half of the revenue lost, so it is the clearest place to focus retention offers.
* Month-to-month contracts and the first 12 months of the customer lifetime are where most churn happens, so contract upgrades and onboarding are the main levers.

## Recommendations

* Target the high-risk segment first with retention offers, and pilot them against a control group.
* Offer incentives for moving month-to-month customers onto one or two year contracts.
* Add onboarding check-ins during the first 12 months.
* Test a tech support trial or bundle for at-risk fiber customers.
* Encourage automatic payments with a small incentive.
* Investigate service quality and value for money on fiber plans before changing prices.

## Limitations

* The patterns are associations, not proven causes. Risk factors such as contract type, fiber optic service and electronic check overlap, and the dataset has no data on service quality, complaints or competitor offers.
* The high-risk segment was defined after seeing which factors had the highest churn, so its significance test is descriptive rather than confirmatory. The size of the effect matters more than the p-value.
* Lost revenue is the monthly recurring revenue of customers who have already left, not a forecast.

## Project Files

* `excel/Telecom_Clean_Data.csv` -- Cleaned dataset
* `excel/data_cleaning_log.md` -- Cleaning steps and validation
* `excel/` -- Pivot table screenshots
* `python/` -- Python analysis notebook with charts
* `dashboard/` -- Power BI dashboard (.pbix) and screenshots

Rohnit Bhardwaj
