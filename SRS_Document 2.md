<!-- ========================================================================= -->
<!-- COVER / TITLE PAGE                                                        -->
<!-- ========================================================================= -->

<div align="center">

# HCL COLLEGE TRAINING PROGRAM
## Project 1: Retail Sales Performance Analysis

### SOFTWARE REQUIREMENTS SPECIFICATION (SRS)

<br/>

---

### **STUDENT & SUBMISSION DETAILS**

| Field | Details |
| :--- | :--- |
| **Student Name** | **Prathvi Agarwal** |
| **Roll No.** | **2026B01010436** |
| **Section** | **CSE-10** |
| **Official Email ID** | [prathvi.26b01010436@abes.ac.in](mailto:prathvi.26b01010436@abes.ac.in) |
| **GitHub Repository** | [Retail-sales-performance-analysis](https://github.com/prathvi-agarwal/Retail-sales-performance-analysis) |
| **Institution** | **ABES Engineering College** |
| **Submission Date** | **20 Nov 2026** |
| **Training Course** | HCL College Training Program |
| **Project Title** | Project 1: Retail Sales Performance Analysis |
| **Domain / Industry** | Retail & E-Commerce Data Analytics |
| **Deliverables** | SRS Document, Source Code File (.py / .ipynb), and GitHub Repository Link |

---

<br/>

</div>

<div style="page-break-after: always;"></div>

<!-- ========================================================================= -->
<!-- DOCUMENT METADATA                                                         -->
<!-- ========================================================================= -->

# SOFTWARE REQUIREMENTS SPECIFICATION (SRS)
## Project 1: Retail Sales Performance Analysis

---

**Document Version:** 1.0.0  
**Student Name:** Prathvi Agarwal  
**Roll No.:** 2026B01010436  
**Section:** CSE-10  
**Email:** prathvi.26b01010436@abes.ac.in  
**GitHub Repository:** [Retail-sales-performance-analysis](https://github.com/prathvi-agarwal/Retail-sales-performance-analysis)  
**Institution:** ABES Engineering College  
**Date:** 20 Nov 2026  
**Course / Training Program:** HCL College Training Program  
**Submission Requirement:** Source Code, SRS Document, and GitHub Repository Link  
**Target Platform:** Python 3.x, Pandas, NumPy, Matplotlib, Seaborn  

---

## 1. Introduction

### 1.1 Student & Project Information
| Parameter | Details |
| :--- | :--- |
| **Student Name** | **Prathvi Agarwal** |
| **Roll No.** | **2026B01010436** |
| **Section** | **CSE-10** |
| **Official Email ID** | [prathvi.26b01010436@abes.ac.in](mailto:prathvi.26b01010436@abes.ac.in) |
| **GitHub Repository** | [Retail-sales-performance-analysis](https://github.com/prathvi-agarwal/Retail-sales-performance-analysis) |
| **Institution** | ABES Engineering College |
| **Training Course** | HCL College Training Program |
| **Project Title** | Project 1: Retail Sales Performance Analysis |

### 1.2 Purpose
The purpose of this document is to define the comprehensive Software Requirements Specification (SRS) for the **Retail Sales Performance Analysis System (Project 1)**. This analytics pipeline processes raw transactional data (`01_retail_sales.csv`), addresses data quality discrepancies, performs statistical analysis and outlier detection, and produces actionable business intelligence regarding revenue trends, product line viability, regional customer concentration, and payment channel efficiencies.

### 1.3 Scope of the System
The system is an end-to-end data analytics and decision-support pipeline designed to:
- Ingest and validate transaction records with integrity checks.
- Clean missing values using business-justified customer-level and regional imputation strategies.
- Verify row-wise mathematical consistency against computed pricing formulas.
- Aggregate core financial metrics (Total Revenue, Units Sold, Average Order Value).
- Identify top-performing products, geographic hubs, and high-value customer tiers.
- Detect abnormal and bulk transaction outliers via the Interquartile Range (IQR) technique.
- Generate structured tabular exports and executive visualizations.
- Deliver five evidence-backed strategic business recommendations.

### 1.4 Definitions, Acronyms, and Abbreviations
- **SRS:** Software Requirements Specification
- **AOV:** Average Order Value ($\text{AOV} = \frac{\text{Total Revenue}}{\text{Total Orders}}$)
- **IQR:** Interquartile Range ($\text{IQR} = Q_3 - Q_1$)
- **EDA:** Exploratory Data Analysis
- **SKU:** Stock Keeping Unit
- **LMS:** Learning Management System
- **CSV:** Comma-Separated Values

### 1.5 References
- IEEE Std 830-1998: Recommended Practice for Software Requirements Specifications
- Python Data Science Handbook (Jake VanderPlas)
- Pandas & NumPy Core Documentation

---

## 2. Overall Description

### 2.1 Product Perspective
The Retail Sales Performance Analysis system operates as an autonomous, scriptable Python analytics solution. It bridges the gap between raw, imperfect operational e-commerce transaction logs and executive decision-makers by generating cleansed data artifacts, statistical profiles, and management reporting deliverables.

### 2.2 User Classes & Stakeholders
1. **Executive Leadership / Retail Operations:** Needs high-level summaries of revenue, monthly growth trends, and strategic expansion recommendations.
2. **Inventory & Category Managers:** Requires SKU-level and category-level velocity reports to optimize procurement and prevent stockouts.
3. **Marketing & Customer Experience Teams:** Analyzes customer spending distribution, geographic ranking, and preferred checkout payment methods.
4. **Academic Evaluator / HCL Mentor:** Assesses implementation fidelity against required tasks (Tasks 4–18), code quality, data handling rigor, and repository structure.

### 2.3 Operating Environment
- **Runtime:** Python 3.10+ / 3.15
- **Dependencies:** `pandas`, `numpy`, `matplotlib`, `seaborn`
- **File System:** Standard cross-platform OS (Windows / Linux / macOS)
- **Version Control & Hosting:** Git & GitHub

### 2.4 Design Constraints
- Must execute deterministically without manual user intervention.
- Preserves raw source records by writing output data to `cleaned_retail_sales.csv`.
- Adheres strictly to the 15 required core analytics tasks (Tasks 4 through 18).

---

## 3. Data Dictionary & Schema Specifications

The incoming source dataset `01_retail_sales.csv` contains the following fields:

| Field Name | Data Type | Constraint / Description |
| :--- | :--- | :--- |
| `Order_ID` | String / Object | Unique alphanumeric identifier for each customer transaction (e.g., `ORD-2024001`). |
| `Order_Date` | ISO Date / String | Date of order placement (`YYYY-MM-DD`). |
| `Customer_ID` | String / Object | Unique customer identifier (`CUST-1001` through `CUST-1120`). |
| `City` | String (Categorical) | Primary delivery city; subject to missing data injection (~4.6%). |
| `State` | String (Categorical) | Corresponding state/province of destination city. |
| `Product_Category`| String (Categorical) | Retail category (Electronics, Fashion, Home & Kitchen, etc.). |
| `Product_Name` | String | Specific product model / SKU title. |
| `Quantity` | Integer (Positive) | Number of units purchased in the transaction. |
| `Unit_Price` | Float (2 decimals) | Unit selling price of the item in Indian Rupees (₹). |
| `Revenue` | Float (2 decimals) | Supplied line-item transaction value; subject to validation. |
| `Payment_Method` | String (Categorical) | Checkout payment instrument (Credit Card, UPI, Debit Card, etc.). |

---

## 4. Functional Requirements (Tasks 4 – 18 Mapping)

### FR-01: Data Ingestion & Schema Inspection (Task 4)
- **Description:** System shall load `01_retail_sales.csv`, determine row and column dimensions, and print schema data types.
- **Verification:** Dimension verification confirms 1,200+ transactional records and 11 distinct feature attributes.

### FR-02: Anomaly & Duplicate Detection (Task 5)
- **Description:** System shall identify null/empty fields per column and detect identical duplicate order entries across all columns.
- **Verification:** Automatically detects and purges all redundant duplicate records while logging initial vs. deduplicated counts.

### FR-03: Justified Missing City Imputation (Task 6)
- **Description:** System shall clean missing `City` entries using a tiered, justified data imputation strategy:
  1. *Customer Order History Imputation:* Search historical transactions from the same `Customer_ID` and impute their confirmed residential city.
  2. *Regional Mode Imputation:* Where customer history is unavailable, impute using the statistical mode city of the designated `State`.
  3. *Unspecified Category:* Any remaining unmapped records default to `'Unknown'`.
- **Justification:** Avoids discarding valid commercial revenue rows while preserving geographic integrity.

### FR-04: Temporal Feature Engineering (Task 7)
- **Description:** Parse `Order_Date` to native timestamp format and extract two derived temporal features:
  - `Month` (Full month name, e.g., October, November)
  - `Weekday` (Day of week name, e.g., Friday, Saturday)

### FR-05: Row-Wise Revenue Audit & Reconciliation (Task 8)
- **Description:** Compute verified revenue $\text{Calculated\_Revenue} = \text{Quantity} \times \text{Unit\_Price}$. Flag any variance $|\text{Revenue} - \text{Calculated\_Revenue}| > 0.01$, log discrepancies, and overwrite with the verified calculation.

### FR-06: Macro-Level Financial Summary (Task 9)
- **Description:** Aggregate and output the core business metrics:
  - $\text{Total Revenue} = \sum \text{Revenue}$
  - $\text{Total Units Sold} = \sum \text{Quantity}$
  - $\text{Total Valid Orders} = N$
  - $\text{Average Order Value (AOV)} = \frac{\text{Total Revenue}}{N}$
- **Export:** Save results to `outputs/revenue_summary_table.csv`.

### FR-07: Top-10 Revenue SKU Ranking (Task 10)
- **Description:** Aggregate revenue by `Product_Name` and rank the top 10 revenue-generating products in descending order.
- **Export:** Save results to `outputs/top_10_products.csv`.

### FR-08: Geographic Revenue Ranking (Task 11)
- **Description:** Group transactions by `City`, compute total orders, units sold, total revenue, and percentage share of total business.
- **Export:** Save results to `outputs/city_revenue_ranking.csv`.

### FR-09: Monthly Trend Analysis & Peak Month Identification (Task 12)
- **Description:** Group transactions by calendar month, calculate monthly revenue, order volume, and AOV, and programmatically identify the highest-earning month.
- **Export:** Save results to `outputs/monthly_trend_table.csv`.

### FR-10: Customer Lifetime Value (Top 10 Spenders) (Task 13)
- **Description:** Group by `Customer_ID`, aggregate total revenue spend and order count, and extract the top 10 VIP customers.
- **Export:** Save results to `outputs/top_10_customers.csv`.

### FR-11: Payment Channel Performance Comparison (Task 14)
- **Description:** Compare checkout payment methods across total order count, order percentage share, aggregate revenue, revenue share, and AOV.
- **Export:** Save results to `outputs/payment_method_comparison.csv`.

### FR-12: IQR Outlier Detection on Transaction Values (Task 15)
- **Description:** Utilize NumPy to compute the 25th percentile ($Q_1$), 75th percentile ($Q_3$), and Interquartile Range ($\text{IQR} = Q_3 - Q_1$). Establish the upper threshold $\text{Upper Bound} = Q_3 + 1.5 \times \text{IQR}$. Identify and isolate all bulk/unusually large orders.
- **Export:** Save isolated outlier records to `outputs/iqr_outliers.csv`.

### FR-13: Product Category Breakdown (Task 16)
- **Description:** Execute multi-attribute groupby on `Product_Category` to compute order count, unit volume, total revenue, average unit price, and AOV.
- **Export:** Save results to `outputs/product_category_summary.csv`.

### FR-14: NumPy Statistical Profiling (Task 17)
- **Description:** Use `np.mean()`, `np.median()`, and `np.std()` on order revenue. Contrast Mean against Median to evaluate revenue skewness and distributional kurtosis.

### FR-15: Strategic Business Formulation (Task 18)
- **Description:** Formulate five evidence-based, data-backed business recommendations directly derived from the empirical findings of Tasks 9 through 17.

---

## 5. Non-Functional Requirements

### 5.1 Reliability & Data Integrity
- The system shall guarantee zero data loss during cleaning.
- Reconciliation of discrepancies must be fully auditable and logged.

### 5.2 Performance & Execution Speed
- Processing of 1,200+ transactional records, including statistical calculations and chart rendering, shall complete within 5 seconds on standard desktop hardware.

### 5.3 Maintainability & Code Quality
- Code must follow PEP 8 standards with modular functions, clear type handling, descriptive variable names, and inline docstrings.

---

## 6. Strategic Business Recommendations (Task 18)

Based on empirical data analysis from the execution of the pipeline:

1. **High-Velocity SKU Stock Prioritization:** The Top 10 products (led by Robotic Vacuum Cleaners, 5G Smartphones, and 4K Gaming Monitors) generate over 60% of total revenue. Establish dedicated safety-stock buffers and priority vendor SLAs for these SKUs to prevent revenue-draining stockouts.
2. **Metro Micro-Fulfillment Expansion:** Urban hubs (Bengaluru, Mumbai, Delhi, Pune, Hyderabad) account for dominant regional revenue. Deploy regional micro-warehousing in these key centers to facilitate same-day delivery and decrease cart drop-offs.
3. **VIP Loyalty & Retention Tier:** The Top 10 customers exhibit high purchasing power and multiple repeat orders. Implement a dedicated VIP Tier offering exclusive early access, dedicated account concierge, and tiered cashback rewards to maximize Customer Lifetime Value (CLV).
4. **Demand-Driven Promotional Scheduling:** Revenue peaks sharply during specific high-volume months. Concentrate performance marketing budgets and promotional bundles in the weeks leading up to this peak season to capture consumer purchase intent.
5. **High-Ticket Financing & Zero-Cost EMI:** Orders exceeding the IQR threshold (₹29,205) represent prime high-margin transactions. Integrate co-branded bank partnerships with instant zero-cost EMI checkout options for Credit Card and UPI payments to minimize cart abandonment on premium goods.

---

## 7. Deliverables & Submission Checklist

- [x] Cleaned Dataset: `cleaned_retail_sales.csv`
- [x] Python Script: `retail_sales_analysis.py`
- [x] Jupyter Notebook: `retail_sales_analysis.ipynb`
- [x] Software Requirements Specification: `SRS_Document.md`
- [x] Output Summary Tables: `outputs/*.csv`
- [x] Analytical Visual Charts: `charts/*.png`
- [x] Complete GitHub Repository: `README.md` and repository files ready for LMS submission.
