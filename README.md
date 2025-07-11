# Payment analysis in Ewallet service Using Python

<img width="1920" height="1080" alt="Cost_to_Build_a_Digital_Wallet_App_Like_Payit_Banner_60d50a9937" src="https://github.com/user-attachments/assets/1dc37ed7-a6ff-42bc-b3c1-ec4071616d75" />

Author: Nguyen Anh Huy

Date: 02/05/2025

Tools Used: Python (Jupiter Notebook)

## 📑 Table of Contents

[📘 Project Overview: E-Wallet Transaction & Payment Analysis](https://github.com/yuhanguyen1/Payment-analysis-in-Ewallet-service-Using-Python?tab=readme-ov-file#-project-overview-e-wallet-transaction--payment-analysis)

[📂 Dataset Description & Access](https://github.com/yuhanguyen1/Payment-analysis-in-Ewallet-service-Using-Python?tab=readme-ov-file#-dataset-description--access)

[🧪 Main Process](https://github.com/yuhanguyen1/Payment-analysis-in-Ewallet-service-Using-Python?tab=readme-ov-file#-main-process)

[🔎 Final Conclusion & Recommendations](https://github.com/yuhanguyen1/Payment-analysis-in-Ewallet-service-Using-Python?tab=readme-ov-file#-recommendations)

## 📘 Project Overview: E-Wallet Transaction & Payment Analysis

### 📌 What is this project about?

This project uses **Python** and **Pandas** to analyze payment and transaction activity within an **e-wallet system**. The main goal is to extract valuable business insights from internal payment data, identify anomalies, and understand user and product behaviors.

---

### 🧠 Business Questions Addressed

- 📈 What are the key **trends in transaction and payment volume** over time?
- 🚩 Are there any **anomalies in team or product performance**?
- 💳 Which **products or teams contribute most** to refunds or transaction volume?
- 🧾 Can we **categorize transactions** into useful types for further behavioral analysis?

---

### 👥 Who is this project for?

This project is designed to support:

- 📊 **Data Analysts & Business Analysts** — to explore operational and financial behavior.
- 🏦 **Fintech Stakeholders & Decision-Makers** — for strategy, risk, and refund insights.
- 🧪 **Product Teams** — to evaluate product performance and service quality in the e-wallet ecosystem.

---

## 📂 Dataset Description & Access

### 📌 Data Source

- **Source**: Internal company database  
- **Format**: CSV files  
- **Files Used**:
  - `product.csv` — 493 rows × 3 columns
  - `payment_report.csv` — 920 rows × 5 columns
  - `transactions.csv` — 1,324,002 rows × 9 columns

---

### 📊 Data Structure & Relationships

#### 1️⃣ Datasets Used

| File Name | Description |
|-----------|-------------|
| `product.csv` | Contains product information, such as category and responsible team |
| `payment_report.csv` | Monthly records of payment volume by product and source |
| `transactions.csv` | Full transaction-level data, including payment details and metadata |

#### 🔗 Key Relationships

- `product_id` links **`product.csv`** and **`payment_report.csv`**
- `transaction_id` is the unique identifier in **`transactions.csv`**
- `source_id` in `payment_report.csv` contributes to refund source analysis

---

#### 2️⃣ Table Schema & Data Snapshot

<details>
<summary>📄 Table 1: <code>product.csv</code> – Product Information (493 rows)</summary>

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `product_id` | INT | Unique product identifier |
| `category` | TEXT | Category the product belongs to |
| `team_own` | TEXT | Team responsible for the product |

</details>

<details>
<summary>📄 Table 2: <code>payment_report.csv</code> – Payment Summary (920 rows)</summary>

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `report_month` | DATE | Reporting month of payment |
| `payment_group` | TEXT | Type of payment (e.g., refund, purchase) |
| `product_id` | INT | Product associated with the payment |
| `source_id` | INT | Source of the transaction |
| `volume` | FLOAT | Total payment volume |

</details>

<details>
<summary>📄 Table 3: <code>transactions.csv</code> – Transaction Records (1,324,002 rows)</summary>

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `transaction_id` | INT | Unique transaction identifier |
| `merchant_id` | INT | Merchant involved in the transaction |
| `volume` | FLOAT | Transaction amount |
| `transType` | INT | Type of transaction (e.g., send, receive, refund) |
| `transStatus` | TEXT | Transaction status (e.g., completed, failed) |
| `sender_id` | INT | Sender’s unique ID |
| `receiver_id` | INT | Receiver’s unique ID |
| `extra_info` | TEXT | Additional metadata |
| `timeStamp` | TIMESTAMP | Timestamp of the transaction |

</details>

---


## 🧪 Main Process

In this project, I conducted two main phases: **Exploratory Data Analysis (EDA)** to assess data quality and structure, and **Data Wrangling** to extract actionable insights from transaction and payment behaviors.

---

### 🔍 Part I: Exploratory Data Analysis (EDA)

---

### 📌 Step 1: Load and merge datasets  
> 🎯 Load `transactions.csv`, `payment_report.csv`, and `product.csv`, then merge `payment_report` and `product` using `product_id` to create a comprehensive payment dataset (`payment_enriched`).
```python
import pandas as pd
import numpy as np

# define payment_enriched, transactions
transactions = pd.read_csv('transactions.csv')
payment_report = pd.read_csv('payment_report.csv')
product = pd.read_csv('product.csv')

payment_enriched = pd.merge(payment_report, product, on="product_id")
```

---

### 📌 Step 2: Inspect data types and structure  
> 🎯 Check data types and column formats for both `payment_enriched` and `transactions` to ensure fields like dates and IDs are in the correct format for analysis.
```python
# Checking Data types
payment_enriched.info()
transactions.info()
```

<img width="492" height="536" alt="image" src="https://github.com/user-attachments/assets/1773a32c-5ed2-48e1-b79b-9e8bed0bcfce" />

---

### 📌 Step 3: Convert and standardize data types  
> 🎯 Convert `report_month` to datetime, and convert `product_id` and `source_id` to strings to support grouping and plotting.
```python
# Convert report_month to datetime type
payment_enriched['report_month'] = pd.to_datetime(payment_enriched['report_month'], format = '%Y-%m')

# Convert product_id & source_id to string for iteration
payment_enriched['product_id'] = payment_enriched['product_id'].astype(str)
payment_enriched['source_id'] = payment_enriched['source_id'].astype(str)
```

---

### 📌 Step 4: Check for missing values  
> 🎯 Identify null values in fields such as `sender_id`, `receiver_id`, and `extra_info` in `transactions`, and determine if any handling is required for incomplete records.
```python
# Checking Missing values
print(payment_enriched.isnull().sum())
print(transactions.isnull().sum())
```

<img width="264" height="318" alt="image" src="https://github.com/user-attachments/assets/e158d0e0-e826-494c-939a-a14b21c5a39f" />

---

### 📌 Step 5: Check for duplicate records  
> 🎯 Confirm uniqueness of records in both datasets to ensure there are no duplicated rows that could skew analysis results.
```python
# Checking duplicates
print(transactions.duplicated())
print(payment_enriched.duplicated())
```

<img width="358" height="425" alt="image" src="https://github.com/user-attachments/assets/b5733922-e9c1-4832-9518-7c67464f7e9a" />

---

### 📌 Step 6: Summarize numerical data  
> 🎯 Use `.describe()` to identify unusual or extreme values in fields like `volume`, `merchant_id`, or `transStatus` for potential data anomalies.
```python
# Check numeric values
print(transactions.describe())
```

<img width="674" height="349" alt="image" src="https://github.com/user-attachments/assets/3adc0095-d4e2-4868-8e45-676402e83a5c" />

---

### 🧹 Part II: Data Wrangling

---

### 📌 Step 7: Top 3 products by total volume  
> 🎯 Group payment data by `product_id` to identify the top 3 products with the highest transaction volume across the dataset.
```python
# group volume by product_id
df_top3 = payment_report.groupby('product_id')['volume'].sum()

# sort value decending
df_top3 = df_top3.sort_values(ascending=False)

# print top 3
print(df_top3.head(3))
```

<img width="234" height="97" alt="image" src="https://github.com/user-attachments/assets/ef7fd3c8-cb56-4983-b279-b585782c8957" />

---

### 📌 Step 8: Check ownership consistency  
> 🎯 Verify that each `product_id` is managed by only one `team_own` as per business rules, and detect any exceptions or mismatches in team assignment.
```python
# group team by product_id
df_product_team = payment_enriched.groupby('product_id', as_index=False)['team_own'].nunique()

# abnormal products
print(df_product_team[df_product_team['team_own'] != 1])
```

<img width="254" height="70" alt="image" src="https://github.com/user-attachments/assets/b9aae7f4-61ca-4790-a649-50578c99d042" />

---

### 📌 Step 9: Identify lowest-performing team and category  
> 🎯 Filter payments since Q2 2023, then group by `team_own` to find the team with the lowest transaction volume, and further break down by `category` to find underperforming product lines.
```python
# filter value since 2023-04
df_team_volume = payment_enriched[(payment_enriched['report_month'] >= '2023-04-01')]

# group volume by team_own
df_team_volume = df_team_volume.groupby('team_own')['volume'].sum()

# sorting value
df_team_volume = df_team_volume.sort_values(ascending=True)

print(df_team_volume.head(1))

# filter volume & category by team APS and since 2023-04
df_aps = payment_enriched[(payment_enriched['team_own'] == 'APS') & (payment_enriched['report_month'] >= '2023-04-01')]

# group volume by category
df_aps = df_aps.groupby('category')['volume'].sum()

# sorting value
df_aps = df_aps.sort_values(ascending=True)

# find the category with lowest volume
print(df_aps.head(1))
```

<img width="232" height="122" alt="image" src="https://github.com/user-attachments/assets/ff0e3d04-795d-4c9f-8217-e771d0cc1312" />

---

### 📌 Step 10: Analyze refund sources  
> 🎯 Filter `payment_group = refund` and group by `source_id` to identify which source contributes most to refund volume, aiding in risk and operational insights.
```python
# filter value for refund transactions
df_refund = payment_report[payment_report['payment_group'] == 'refund']

# group source_id by sum
df_refund = df_refund.groupby('source_id')['volume'].sum()

# sort value decending
df_refund = df_refund.sort_values(ascending=False)

print(df_refund.head(1))
```

<img width="275" height="78" alt="image" src="https://github.com/user-attachments/assets/b9425668-3a2d-4e9a-8c41-c0ac27573256" />

---

### 📌 Step 11: Classify transaction types  
> 🎯 Create a new column `transaction_type` in `transactions.csv` by mapping combinations of `transType` and `merchant_id` into business-meaningful categories like "Top Up", "Withdraw", or "Transfer".
```python
# df transaction_type
def transaction_type(row):
  if row['transType'] == 2 and row['merchant_id'] == 1205:
    return 'Bank Transfer Transaction'
  elif row['transType'] == 2 and row['merchant_id'] == 2260:
    return 'Withdraw Money Transaction'
  elif row['transType'] == 2 and row['merchant_id'] == 2270:
    return 'Top Up Money Transaction'
  elif row['transType'] == 2:
    return 'Payment Transaction'
  elif row['transType'] == 8 and row['merchant_id'] == 2250:
    return 'Transfer Money Transaction'
  elif row['transType'] == 8:
    return 'Split Bill Transaction'
  else:
    return 'Invalid'
transactions['transaction_type'] = transactions.apply(transaction_type, axis=1)

print(transactions[['transType', 'merchant_id' ,'transaction_type']])
```

<img width="533" height="261" alt="image" src="https://github.com/user-attachments/assets/d20893cf-2e71-4282-ad8d-8d04bb7daaab" />

---

### 📌 Step 12: Aggregate metrics by transaction type  
> 🎯 For each valid `transaction_type`, calculate total number of transactions, total volume, number of unique senders, and receivers to uncover user behavior patterns and product usage.
```python
# filter data to exclude invalid transaction type
transactions = transactions[transactions['transaction_type'] != 'Invalid']

transactions = transactions.groupby('transaction_type').agg({'transaction_id': 'count', 'volume': 'sum', 'sender_id': 'nunique', 'receiver_id': 'nunique'})
print(transactions)
```

<img width="631" height="308" alt="image" src="https://github.com/user-attachments/assets/88fd6e59-765e-4a86-9fed-8513ccf2fc5e" />

---

## 🔎 Final Conclusion & Recommendations

### 🔗 Findings:

1. **Top 3 products with the highest volume:**

   + Product 1976:    61797583647 products
   + Product 429:     14667676567 products
   + Product 372:    13713658515 products
3. **Anomalies in team or product performance:**

   + No abnormal products (each product is handled by one team).
  
3. **Lowest Performance Team in Q2.2023:**
   + Team APS with volume = 51141753
   + Category PXXXXXB contributes 25232438 products.

4. **Highest Contributor in Refund Transactions:**

   + Source ID 38:    36527454759 products
  
5.  **Transaction Types:**

     + Bank Transfer: 37,879 transactions, volume = 50.6B
     + Payment: 398,677 transactions, volume = 71.85B
     + Split Bill: 1,376 transactions, volume = 4.9M
     + Top Up: 290,502 transactions, volume = 108.6B
     + Transfer: 341,177 transactions, volume = 37B
     + Withdraw: 33,725 transactions, volume = 23.42B


### 📈 Recommendations:

1. **Focus on the Product**:

 Product ID 1976 should be given priority because it generates the most volume.
 Look into ways to increase the low volume of Product ID 372.

 2. **The APS team**:

 In Q2.2023, APS performs the worst.  Examine and improve their performance, particularly in the PXXXXXB category, which only contributes 36527454759 products.
 
 3. **Refunds**:

 The largest contributor to refunds is Source ID 38 (36527454759 products). Examine any possible problems with this source in order to lower the volume of refunds.
 
 4. **Optimization of Transactions**:

 Since they have the highest volume, concentrate on making the Payment and Top Up transactions better.
 To increase volume, think about boosting engagement for split bill transactions.
 
 5. **Engagement of Senders and Receivers**:

 Focus on improving the sender and recipient experience, particularly for high-volume transactions like Top Up and Payment.
