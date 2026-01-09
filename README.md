# Credit-Card-Fraud-Detection

## Project Description

Credit card fraud remains one of the most significant challenges faced by financial institutions worldwide. According to Nilson (2021), global losses from credit card fraud reached **$28.58 billion in 2020**. This project focuses on applying **feature engineering techniques** to raw credit card transaction data to improve fraud detection accuracy.  

The dataset includes transaction-level details such as timestamps, merchant identifiers, transaction amounts, customer demographics, and geolocation data. By deriving additional, meaningful features from these raw inputs, the objective is to better understand behavioral patterns linked to fraudulent activity and strengthen fraud prevention measures.

---

## Objective

The project aims to:
1. Engineer meaningful features that improve the identification of fraudulent transactions.  
2. Understand key indicators of abnormal or suspicious transaction behavior.  
3. Derive business insights that can help financial institutions design stronger fraud prevention strategies.  

---

## Methodology

### Step 1: Data Import and Preparation
- Imported raw transaction data using **pandas**.  
- Parsed the transaction time and date of birth columns to proper datetime formats.  
- Sorted the dataset chronologically to maintain sequence accuracy.

### Step 2: Feature Inspection
- Used `DataFrame.info()` to review variable types and detect missing or inconsistent data.  
- Examined descriptive statistics to understand transaction distribution, amount ranges, and time variability.

### Step 3: Feature Engineering
#### 1. Age Extraction
Added a customer age column derived from the date of birth:
```python
fraud_df["age"] = fraud_df["dob"].apply(lambda x: (date.today().year - x.year - ((date.today().month, date.today().day) < (x.month, x.day))))
```
Age can influence fraud risk, as certain age groups may be more vulnerable to online scams or card misuse.

#### 2. Distance from Merchant
Calculated the physical distance between the customer’s and merchant’s locations using geodesic distance:
```python
fraud_df["distance_from_merchant"] = fraud_df.apply(lambda x: distance.geodesic((x["lat"], x["long"]), (x["merch_lat"], x["merch_long"])).km, axis=1)
```
Transactions made far from the customer’s usual area may indicate compromised card usage.

#### 3. Domestic vs. International Transactions
Created a flag to identify if a transaction occurred within the same country:
```python
fraud_df["is_domestic"] = fraud_df["customer_country"] == fraud_df["merchant_country"]
```
International transactions can be a potential indicator of fraud, especially when inconsistent with customer history.

---

## Data Overview

| Variable | Description | Type | Example |
|-----------|--------------|------|----------|
| trans_date_trans_time | Timestamp of the transaction | Datetime | 2020-03-01 08:45:21 |
| dob | Customer date of birth | Datetime | 1985-07-13 |
| lat / long | Customer latitude and longitude | Float | 37.77, -122.42 |
| merch_lat / merch_long | Merchant latitude and longitude | Float | 40.75, -73.99 |
| customer_country | Country of the customer | String | "USA" |
| merchant_country | Country of the merchant | String | "USA" |
| amount | Transaction amount in USD | Float | 52.10 |
| age | Derived feature: customer age | Integer | 39 |
| distance_from_merchant | Derived feature: distance in kilometers | Float | 150.2 |
| is_domestic | Derived feature: domestic transaction flag | Boolean | True |

---

## Insights

1. **Transactions made at large distances** from a customer's usual spending region were found to have a higher fraud probability.  
2. **Cross-border transactions** had a noticeably larger share of fraudulent activity compared to domestic ones.  
3. **Customer age distribution** revealed that both younger (18–25) and older (>60) customers showed higher susceptibility to fraudulent charges.  
4. **High-value transactions** made outside usual hours (e.g., late night or early morning) often correlated with fraudulent cases.

---

## Suggestions

1. **Enhance transaction monitoring** by incorporating geo-verification alerts for high-distance or cross-border purchases.  
2. **Implement customer-specific behavioral thresholds**, such as maximum transaction distance or time-based spending limits, to detect anomalies early.  
3. **Use dynamic spending alerts** that notify customers of transactions made in new regions or through unfamiliar merchants.  
4. **Promote customer awareness programs** on online and card-not-present fraud prevention, particularly for vulnerable age groups.  
5. **Encourage two-factor authentication (2FA)** for international or high-value transactions to prevent unauthorized access.  
6. **Collaborate with merchants** to ensure that suspicious transactions are flagged in real time and validated before approval.  
7. **Regularly review and update blacklists** of known fraudulent merchants or geographic locations with a history of fraudulent activity.  

---
## Conclusion and Recommendations

Through this project, I explored how feature engineering can enhance fraud detection by creating meaningful variables from raw credit card transaction data. Key insights include:

**Customer Spending Behavior:**

Features such as total transactions, average transaction amounts, and daily/30-day spending trends highlight unusual activity.

Sudden spikes in transaction frequency or unusually high daily spending can indicate potential fraud.

**Merchant and Category Risk:**

Calculating the proportion of fraudulent transactions per merchant (merch_fraud_prop) and per category (category_fraud_prop) revealed that certain merchants or categories are historically more prone to fraud. Transactions at new or high-risk merchants/categories can serve as early warning signs.

**Transaction Context Features:**

Distance from merchant and domestic vs. international transactions provide behavioral context. Transactions made far from a customer’s usual locations or in other countries are more likely to be suspicious.

**Based on these insights, the following recommendations can help financial institutions and customers mitigate fraud risk:**

Real-Time Monitoring and Alerts: Set up alerts for unusual activity, such as large purchases at unfamiliar merchants, cross-border transactions, or sudden changes in daily spending patterns.

Customer Awareness Programs: Educate customers about common fraud signals and encourage them to monitor transactions regularly.

High-Risk Merchant/Category Tracking: Use historical fraud proportions to flag merchants or categories with higher risk, and prioritize review or verification for transactions involving them.

Behavioral Thresholds: Implement adaptive thresholds for spending and frequency per customer to detect deviations from normal behavior.

Two-Factor Authentication: For high-risk transactions, especially international or high-value purchases, require additional verification to prevent unauthorized use.

Overall, this project demonstrates how structured feature engineering—combining customer history, transaction patterns, and merchant/category risk—can provide actionable insights for fraud detection and prevention. The features developed here can serve as a foundation for predictive models or real-time monitoring systems to help financial institutions safeguard their customers and reduce financial losses.







