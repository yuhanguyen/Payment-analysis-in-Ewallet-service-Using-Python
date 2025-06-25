# Payment analysis in Ewallet using Python
Used Python to perform EDA & Data Wrangling tasks on the given payment dataset.

Author: Nguyen Anh Huy

Date: 02/05/2025

Tools Used: Python (Jupiter Notebook)

## Table of Contents

[I. Background & Overview](https://github.com/yuhanguyen/EDA-and-Data-Wrangling-with-Python/edit/main/README.md#i-background--overview)

[II. Dataset Description & Data Structure](https://github.com/yuhanguyen/EDA-and-Data-Wrangling-with-Python/edit/main/README.md#ii-dataset-description--data-structure)

[III. Main Process](https://github.com/yuhanguyen/EDA-and-Data-Wrangling-with-Python/edit/main/README.md#iii-main-process)

## I. Background & Overview

### Objective:

This project uses Python to analyze the payment dataset from an E-wallet to understand the status of a payment or transaction in the context of the E-wallet.

### Who is this project for?

✔️ Data analysts & business analysts

✔️ Product manager & stakeholders

## II. Dataset Description & Data Structure

The dataset consists of three tables: payment_report.csv (monthly payment volume of products), product.csv (product information), transactions.csv (transactions information). Here is the Data dictionary:

**payment_report.csv**

| Column | Data type | Description |
| :---: | :---: | :---: |
| report_month | OBJECT | report month |
| payment_group | OBJECT | type of payment |
| product_id | INTEGER | unique identifier for each product |
| source_id | INTEGER | product source |
| volume | INTEGER | product volume |

**product.csv**

| Column | Data type | Description |
| :---: | :---: | :---: |
| product_id | INTEGER | unique identifier for each product |
| category | OBJECT | product category |
| team_own | OBJECT | the team that sell the product |

**transactions.csv**

| Column | Data type | Description |
| :---: | :---: | :---: |
| transaction_id | INTEGER | unique identifier for each transaction |
| merchant_id | INTEGER | unique identifier for each merchant |
| volume | INTEGER | product volume |
| transType | INTEGER | type of transaction |
| transStatus | INTEGER | transaction status |
| sender_id | FLOAT | unique identifier for each sender |
| receiver_id | FLOAT | unique identifier for each receiver |
| timeStamp | INTEGER | timestamp of transaction |

## III. Main Process

### 1. EDA

**Import Packages, Dataset**

![image](https://github.com/user-attachments/assets/0704f0eb-2757-47e5-8367-802c01411b71)

**Check data types**

![image](https://github.com/user-attachments/assets/adc6409d-ffc6-41b0-ac28-5483f0757e83)

**Convert report_month to datetime type**

![image](https://github.com/user-attachments/assets/10e58288-4380-44cf-ba1f-e4c8163e67dc)

**Checking Missing values**

![image](https://github.com/user-attachments/assets/bd2833ee-735e-4c2f-937d-954abaad0fed)

**Checking duplicates**

![image](https://github.com/user-attachments/assets/574c5809-e302-40e5-8f97-13d75d611fd2)

**Check numeric values**

![image](https://github.com/user-attachments/assets/273ced2e-407f-440e-9b52-c9764bbb8e8c)


### 2. Data Wrangling

In this part, i will perform 6 data wrangling tasks on the given dataset, as below:

**1. Top 3 product_ids with the highest volume.**

![image](https://github.com/user-attachments/assets/03387944-a455-43c1-9074-7f0372bb6a07)


**2. Given that 1 product_id is only owed by 1 team, are there any abnormal products against this rule?**

![image](https://github.com/user-attachments/assets/526ad0b8-2cd6-4ebe-9f50-ff05763963f3)


**3. Find the team has had the lowest performance (lowest volume) since Q2.2023. Find the category that contributes the least to that team.**

![image](https://github.com/user-attachments/assets/4da15f27-4b43-4871-aff3-86949242dc69)


**4. Find the contribution of source_ids of refund transactions (payment_group = ‘refund’), what is the source_id with the highest contribution?**

![image](https://github.com/user-attachments/assets/2a855170-4b89-435b-8869-281f57d1333c)


**5. Using transactions.csv Define type of transactions (‘transaction_type’) for each row, given:**

**transType = 2 & merchant_id = 1205: Bank Transfer Transaction**

**transType = 2 & merchant_id = 2260: Withdraw Money Transaction**

**transType = 2 & merchant_id = 2270: Top Up Money Transaction**

**transType = 2 & others merchant_id: Payment Transaction**

**transType = 8, merchant_id = 2250: Transfer Money Transaction**

**transType = 8 & others merchant_id: Split Bill Transaction**

**Remained cases are invalid transactions**

![image](https://github.com/user-attachments/assets/a1921a51-1f25-485a-b954-e19f8ebf5c85)


**6. Of each transaction type (excluding invalid transactions): find the number of transactions, volume, senders and receivers.**

![image](https://github.com/user-attachments/assets/eb482f19-5c7f-4902-875a-aee97ffef5fe)

