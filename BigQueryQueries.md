### SQL Queries Overview

#### **Data Extraction:**

#### 1. **Select Addresses with First Transactions in 2023**:

```sql
CREATE TABLE `my-project-444222.wallets_2023.all_new_wallets_2023` AS
WITH first_transactions_all_time AS 
(
  SELECT 
    from_address,
    block_timestamp AS first_transaction_time,
    ROW_NUMBER() OVER (PARTITION BY from_address ORDER BY block_timestamp) AS trans_numbs
  FROM 
    `bigquery-public-data.crypto_ethereum.transactions`
)
SELECT
  from_address,
  first_transaction_time
FROM 
  first_transactions_all_time
WHERE
  trans_numbs = 1
  AND DATE(first_transaction_time) BETWEEN '2023-01-01' AND '2023-12-31';
```

#### 2. **Create a 1% Random Sample and Select 20,000 Rows**:

```sql
CREATE TABLE `my-project-444222.wallets_2023.sample_new_wallets_2023` AS
  SELECT *
  FROM `my-project-444222.wallets_2023.all_new_wallets_2023`
  TABLESAMPLE SYSTEM(1 percent)
  LIMIT 20000;
```

#### 3. **Join the Sample with Additional Columns**:

```sql
WITH ranked_transactions AS (
  SELECT *,
    ROW_NUMBER() OVER (PARTITION BY from_address ORDER BY block_timestamp ASC) AS rank
  FROM `bigquery-public-data.crypto_ethereum.transactions`
  WHERE DATE(block_timestamp) BETWEEN '2023-01-01' AND '2023-12-31'
)
SELECT
  sampled.from_address,
  sampled.first_transaction_time,
  transactions.to_address,
  transactions.value / 1e18 AS eth_value,
  transactions.gas,
  transactions.gas_price,
  transactions.hash,
  transactions.transaction_type,
  transactions.receipt_effective_gas_price,
  transactions.receipt_gas_used
FROM 
  `my-project-444222.wallets_2023.sample20k` AS sampled
JOIN ranked_transactions AS transactions
ON 
  sampled.from_address = transactions.from_address
  AND transactions.block_timestamp = sampled.first_transaction_time
WHERE transactions.rank = 1;
```
---

#### Sample Adequacy Verification:

#### 4. **Calculate the Percentage of Contract Addresses**:  
Determine the percentage of contract addresses (`to_address` with NULL values) in the full dataset and in the sample to ensure representativeness.

- **for population:**

```sql
WITH add_to_address AS (
  SELECT 
    wallets.from_address,
    wallets.first_transaction_time,
    transactions.to_address
  FROM 
    `my-project-444222.wallets_2023.all_new_wallets_2023` AS wallets
  JOIN (
    SELECT from_address, to_address, block_timestamp
    FROM `bigquery-public-data.crypto_ethereum.transactions`
    WHERE DATE(block_timestamp) BETWEEN '2023-01-01' AND '2023-12-31'
  ) AS transactions
  ON 
    wallets.from_address = transactions.from_address
    AND transactions.block_timestamp = wallets.first_transaction_time
)
SELECT 
  COUNT(*) AS total_transactions,
  SUM(CASE WHEN to_address IS NULL THEN 1 ELSE 0 END) AS smart_contract_transactions,
  (SUM(CASE WHEN to_address IS NULL THEN 1 ELSE 0 END) / COUNT(*)) * 100 AS smart_contract_percentage
FROM add_to_address;
```

- **for sample:**

```sql
SELECT 
  COUNT(*) AS total_transactions,
  SUM(CASE WHEN to_address IS NULL THEN 1 ELSE 0 END) AS smart_contract_transactions,
  (SUM(CASE WHEN to_address IS NULL THEN 1 ELSE 0 END) / COUNT(*)) * 100 AS smart_contract_percentage
FROM `my-project-444222.wallets_2023.new_crypto_ethereum_sample20k_new_wallets_2023`;
```

#### 5. **Calculate the Percentage of Zero-Value Transactions**:  
Compare the percentage of zero-value transactions in the full dataset and in the sample to verify alignment within an acceptable margin of error.

- **for population:**

```sql
WITH add_value AS (
  SELECT 
    wallets.from_address,
    wallets.first_transaction_time,
    transactions.value
  FROM 
    `my-project-444222.wallets_2023.all_new_wallets_2023` AS wallets
  JOIN (
    SELECT from_address, value, block_timestamp
    FROM `bigquery-public-data.crypto_ethereum.transactions`
    WHERE DATE(block_timestamp) BETWEEN '2023-01-01' AND '2023-12-31'
  ) AS transactions
  ON 
    wallets.from_address = transactions.from_address
    AND transactions.block_timestamp = wallets.first_transaction_time
)
SELECT 
  COUNT(*) AS total_transactions,
  SUM(CASE WHEN value = 0 THEN 1 ELSE 0 END) AS smart_contract_transactions,
  (SUM(CASE WHEN value = 0 THEN 1 ELSE 0 END) / COUNT(*)) * 100 AS smart_contract_percentage
FROM add_value;
```

- **for sample:**

```sql
SELECT 
  COUNT(*) AS total_transactions,
  SUM(CASE WHEN value = 0 THEN 1 ELSE 0 END) AS smart_contract_transactions,
  (SUM(CASE WHEN value = 0 THEN 1 ELSE 0 END) / COUNT(*)) * 100 AS smart_contract_percentage
FROM `my-project-444222.wallets_2023.new_crypto_ethereum_sample20k_new_wallets_2023`;
```
