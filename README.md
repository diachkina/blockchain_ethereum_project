# **Ethereum Transaction Analysis**

## This project analyzes Ethereum transactions, focusing on the behavior of newly created wallets, gas usage, and interactions with smart contracts in 2023. The study provides insights into transaction types, contract functionalities, and gas efficiency, highlighting key patterns and statistical differences.
---

### Resources

You can access the data files here:
[Google Drive Link](https://drive.google.com/drive/folders/1cIBZGCgxEp5DO4Y9tfp_EI8fi9JBoO8t?usp=sharing)

---

[Tableau Dashboard](https://public.tableau.com/app/profile/tetiana.diachkina/viz/Book2_17372952521110/Dashboard1)

---

## **Table of Contents**
1. [Project Overview](#project-overview)
2. [Technologies Used](#technologies-used)
3. [Key Features](#key-features)
4. [Data Overview](#data-overview)
5. [Methodology](#methodology)
6. [Key Findings](#key-findings)
7. [Installation](#installation)

---

## **Project Overview**

Ethereum's dynamic ecosystem offers diverse transaction types and functionalities. This project examines:
- The first transactions of **newly created wallets** in 2023.
- Transaction classifications, gas usage analysis, and contract functionality breakdown.
- Statistical tests to assess differences in gas efficiency between **stablecoins** and **token (NFT) contracts**.

The project uses **Python** and **BigQuery** to process and analyze transaction data, focusing on extracting actionable insights and visualizing trends.

---

## **Technologies Used**
- **Programming Language**: Python
- **Libraries**:
  - Data Manipulation: `pandas`, `numpy`
  - Visualization: `matplotlib`, `seaborn`
  - Statistical Analysis: `scipy`
- **Cloud Services**: Google BigQuery (for data querying)
- **Preprocessing Data**: SQL (for data querying in Google BigQuery)
- **Development Environment**: Jupyter Lab

---

## **Key Features**
1. **Transaction Type Classification**:
   - Categorized transactions into **Wallet-to-Wallet**, **Wallet-to-Contract**, and **Contract**.

2. **Gas Usage Analysis**:
   - Explored estimated and actual gas usage by transaction type.
   - Identified patterns in gas efficiency for different contract categories.

3. **Contract Functionality**:
   - Mapped the top 25 contract addresses and categorized them into:
     - **Stablecoin**
     - **Router**
     - **Bridge**
     - **Token (NFT)**
     - **Infrastructure**

4. **Statistical Insights**:
   - Applied the **Mann-Whitney U Test** to assess differences in gas estimation accuracy between stablecoin and token contracts.

5. **Data Visualizations**:
   - Created bar charts, histograms, and KDE plots to visualize trends and distributions.

---

## **Data Overview**

The dataset consists of:
- **25+ million transactions** from newly created wallets in 2023.
- A **1% random sample** (252,700 transactions) was used for scalability.
- A further sample of **20,000 transactions** was taken for detailed analysis.

Columns include:
- `from_address`, `to_address`, `value`, `gas`, `gas_price`, `transaction_type`, and more.

---

## **Methodology**
1. **Data Extraction**:
   - Queried Ethereum transaction data from Google BigQuery.
   - Filtered transactions to include only **first outgoing transactions** from new wallets.
   - Verified the adequacy of the sample by calculating the number of nulls and zero-value transactions. The values matched within a margin of error of 0.5%.
   - All SQL queries used for data extraction and verification are saved in the [`BigQueryQueries.md`](./BigQueryQueries.md) file in this repository.

2. **Data Processing**:
   - Converted `value` from Wei to ETH.
   - Categorical classification of transaction purposes and contract functionalities.

3. **Analysis**:
   - Gas usage comparison for transaction types.
   - Statistical tests on gas difference for stablecoins and token contracts.

4. **Visualization**:
   - Plotted distribution of gas usage, transaction values, and contract functionality.

---

## **Key Findings**
- **New Wallet Behavior**:
  - Over 50% of first transactions interact with **smart contracts**.
  - Contract-initiated transactions are rare.

- **Gas Usage**:
  - Wallet-to-Wallet transactions are predictable, while Wallet-to-Contract transactions show variability due to complex contract logic.

- **Contract Functionality**:
  - Stablecoins dominate the top 25 contract addresses, followed by bridges and routers.

- **Statistical Analysis**:
  - Stablecoin contracts overestimate gas requirements compared to token contracts.

---

## **Installation**

To run this project locally, follow these steps:

1. Clone the repository:
   ```bash
   git clone https://github.com/your_username/ethereum-transaction-analysis.git
   cd ethereum-transaction-analysis





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

---

#### **c. Approach to Problem Solving**
1. **Data Collection**:
   - SQL queries were executed in BigQuery to create a base table with the first transactions of new wallets.
   - Data was collected via the Etherscan API to analyze transaction details.

2. **Data Processing**:
   - Tables from BigQuery was supplemented with API info to get complete transaction information.

3. **Transaction Categorization**:
   - By purpose: `Wallet-to-Wallet`, `Wallet-to-Contract`, `Contract`.
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
   - **Visualization**: Used `matplotlib`, `seaborn` to create charts and graphs.
   - **Interactive Visualization**: Used `Tableau`.

---

To verify the adequacy of the sample, the number of nulls and zero-value transactions were calculated. 
The values matched within a margin of error of 0.5%.
