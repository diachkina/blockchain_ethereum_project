### **Blockchain Ethereum Analitic Project**

#### **a. Task Formulation**
**Business and Technical Goals**:
The goal of this project is to analyze the first transactions of new wallets in the Ethereum network in 2023, identify key trends in their activity, transaction types, and categorize them. The analysis aims to provide insights into the behavior of new users and their contribution to blockchain operations.

- **Business Objective**: Determine which types of transactions dominate among new wallets and identify trends in their activity.
- **Technical Objective**: Collect data from BigQuery and the Etherscan API, process it, and classify transactions by types and values.

---

#### **b. Data Sources and Description**
1. **Data Sources**:
   - BigQuery (table `new_crypto_ethereum_sample20k_new_wallets_2023`*).
   - Etherscan API (to retrieve transaction histories for specific addresses).
   
2. **Description**:
   - The BigQuery table contains the first outgoing transactions of new wallets, where the wallet address is the `from_address`.
   - Data obtained via the Etherscan API includes both outgoing and incoming transactions for the same addresses.

---

#### **c. Approach to Problem Solving**
1. **Data Collection**:
   - SQL queries were executed in BigQuery to create a base table with the first transactions of new wallets.
   - Data was collected via the Etherscan API to analyze transaction details.

2. **Data Processing**:
   - Tables from BigQuery and the API were merged to get complete transaction information.
   - Transactions were divided into two categories: "incoming" and "outgoing."

3. **Transaction Categorization**:
   - By type: `incoming`, `outgoing`, `smart contract`.
   - By value:
     - Zero: 0 ETH.
     - Micro: <0.001 ETH.
     - Small: 0.001–0.1 ETH.
     - Medium: 0.1–1 ETH.
     - Large: 1–10 ETH.
     - Whale: >10 ETH.

4. **Tools**:
   - **Python**: For data collection (`pandas`, `requests` libraries).
   - **BigQuery**: For SQL queries to prepare the base table.
   - **Visualization**: Used `matplotlib` to create charts and graphs.

---

#### **d. Results and Conclusions**
1. **Analysis Results**:
   - **Transaction Types**:
     - 95.13% of first transactions by new wallets are incoming.
   - **Transaction Value Distribution**:
     - 38.36% of transactions have a zero value in ETH.
     - Only 1.05% of transactions exceed 10 ETH.
   - **Smart Contract Interactions**:
     - 66% of zero-value transactions are unrelated to smart contracts (likely test actions or verification steps).
     - 34% of zero-value transactions interact with smart contracts (e.g., approve, initialize).

2. **Conclusions**:
   - A significant portion of zero-value transactions may indicate testing activity by new users or automated processes (e.g., creating smart contracts).
   - 73% of non-zero transactions are direct ETH transfers between addresses.
   - Data suggests some seasonality in the creation of new wallets.


* The original size of the crypto-ethereum.transactions dataset was 650,000,000 rows. 
Out of those, the data I needed accounted for 25,000,000 rows. 
A random sample of 1% was taken for the analysis, and then the first 20,000 rows were selected from it.

To verify the adequacy of the sample, the number of nulls and zero-value transactions were calculated. 
The values matched within a margin of error of 0.5%.
