# telecom-dv2-platform

#  Telecom Analytics Platform — Data Vault 2.0

##  Overview

This project demonstrates the design and implementation of a **Telecom Data Warehouse** using the **Data Vault 2.0 architecture**.
It integrates multiple telecom data sources and builds scalable ETL pipelines to enable historical tracking, analytics, and business insights.

---

##  Architecture

The solution follows a layered architecture:

```
Sources → Staging (STG) → Data Vault (Hubs, Links, Satellites) → Business Vault → Data Mart → BI
```

###  Data Vault Components

* **Hubs** → Store business keys (Customer, Device, Complaint, etc.)
* **Links** → Represent relationships between entities
* **Satellites** → Store descriptive attributes with full history tracking

---

##  Data Sources

| Source     | Description          |
| ---------- | -------------------- |
| CRM        | Customer information |
| CDR        | Call Detail Records  |
| Payments   | Transactions data    |
| Complaints | Customer complaints  |
| Plans      | Subscription plans   |
| Devices    | Customer devices     |

---

##  Technologies Used

* **Informatica Cloud (IICS)** — ETL Pipelines
* **SQL Server** — Data Warehouse Storage
* **Data Vault 2.0** — Data Modeling
* **Power BI** — Data Visualization
* **Python** — Data generation & preprocessing

---

##  ETL Pipeline

###  Staging Layer (STG)

* Data ingestion from CSV sources
* Data cleaning and standardization
* Validation rules:

  * Not null checks
  * Deduplication
  * Positive value validation
* Reject handling using Router transformation

<img width="924" height="424" alt="WhatsApp Image 2026-05-01 at 9 02 23 PM (1)" src="https://github.com/user-attachments/assets/a6e4ef6d-7242-4dba-a701-c6164135ffb9" />


<img width="1048" height="503" alt="WhatsApp Image 2026-05-01 at 9 02 23 PM (2)" src="https://github.com/user-attachments/assets/d2cafe7c-3c0e-4089-bb30-291540d08d24" />


<img width="1195" height="316" alt="WhatsApp Image 2026-05-01 at 9 02 23 PM" src="https://github.com/user-attachments/assets/9c887024-36df-4b0b-890c-3ae4b996423b" />


---

###  Data Vault Layer

#### Hubs

* `HUB_CUSTOMER`
* `HUB_DEVICE`
* `HUB_COMPLAINT`
* `HUB_PAYMENT`
* `HUB_PLAN`
* `HUB_CALL`
* `HUB_TOWER`
<img width="1730" height="503" alt="hup_tower" src="https://github.com/user-attachments/assets/18389cc6-62f8-4f0f-a871-cfa840340dff" />

<img width="1733" height="499" alt="hup_call" src="https://github.com/user-attachments/assets/33cf3a51-c79b-415d-9ae2-f75e84c80198" />

✔ Hash Keys (MD5)
✔ Insert-only pattern
✔ Source tracking (`rec_src`)

---

#### Links

* `LINK_DEVICE_CUSTOMER`
* `LINK_COMPLAINT_TOWER_CUSTOMER`
* `LINK_PAYMENT_CUSTOMER`
* `LINK_CALL_CUSTOMER_TOWER`

<img width="1711" height="504" alt="link_dev_cust" src="https://github.com/user-attachments/assets/fd2cc401-166a-42e0-bf8d-5375cc131641" />

<img width="1734" height="498" alt="link_call_cust" src="https://github.com/user-attachments/assets/998530c4-15fe-432a-b3ad-dbb422258d6b" />


✔ Relationship modeling
✔ No descriptive data

---

#### Satellites

* `SAT_CUSTOMER_DETAILS`
* `SAT_DEVICE_DETAILS`
* `SAT_COMPLAINT_DETAILS`
* `SAT_PAYMENT_DETAILS`
<img width="1731" height="498" alt="sat_plan" src="https://github.com/user-attachments/assets/3a0d196f-e7b2-459c-9e9d-caadca31dbfc" />

<img width="1733" height="503" alt="sat_dev" src="https://github.com/user-attachments/assets/bd309ff5-cf31-4d43-aa3d-3d3e0f6be780" />

✔ Historical tracking
✔ Change detection using **hashdiff**
✔ Insert-only versioning

---

###  Business Vault

* Derived metrics and business logic
* Example:

  * Customer churn indicators
  * Payment behavior metrics
  * Complaint resolution KPIs

---

###  Data Mart Layer

#### Fact Tables

* `FCT_CALLS`
* `FCT_PAYMENTS`
* `FCT_COMPLAINTS`

#### Dimension Tables

* `DIM_CUSTOMER`
* `DIM_DEVICE`
* `DIM_PLAN`
* `DIM_DATE`
* `DIM_TOWER`

---

##  Dashboards (Power BI)

*  Executive Overview
*  Churn Risk Monitor
*  Network Health Analysis
*  Revenue & Payments

---

##  Key Features

* ✔ Scalable Data Vault 2.0 architecture
* ✔ End-to-end ETL pipeline using Informatica Cloud
* ✔ Full historical tracking (Slowly Changing Data)
* ✔ Data quality layer with reject handling
* ✔ Multi-source data integration
* ✔ Business-ready analytics

---

# Business Insights & KPI Analysis

## KPI 1: Which Customers Are Likely to Churn?

### INSIGHT #1: Prepaid Customers Have the Highest Churn Risk
Most high-risk customers belong to the **Prepaid** segment.  
This suggests that prepaid users are less loyal and more likely to switch providers.

### INSIGHT #2: Customers with Low Tenure Are More Likely to Churn
Many churned customers have very low tenure values (1–10 months).

This indicates that customers are most likely to leave during the early stage of their subscription lifecycle.

### INSIGHT #3: Certain Cities Show Higher Churn Rates
Cities such as:

- Cairo
- Aswan
- Alexandria

show relatively high churn percentages.

This may indicate regional service quality or customer satisfaction issues.

---

## KPI 2: Why Are Customers Likely to Churn?

### INSIGHT #4: High Monthly Charges Increase Churn Risk
Churned customers pay an average monthly charge of approximately **$74**, while active customers pay around **$61**.

This suggests that expensive services or plans may push customers to leave.

### INSIGHT #5: Churned Customers Usually Have Shorter Relationships
Active customers stay for an average of about **38 months**, while churned customers stay only about **18 months**.

This means long-term customers are generally more loyal.

---

## KPI 3: Are Payment Failures Related to Churn?

### INSIGHT #6: Failed Payments Are Strongly Associated with Churn
Churned customers have significantly more failed payments compared to active customers.

- Active customers average: **0.09** failed payments
- Churned customers average: **0.84** failed payments

This indicates that payment issues may be an early warning sign of customer churn.

### INSIGHT #7: Customers with Multiple Failed Payments Are Mostly Churned
Customers with the highest number of failed payments are almost entirely labeled as churned customers.

This suggests financial issues or dissatisfaction with the service.

---

## KPI 4: Are Dropped Calls and Tower Issues Affecting Satisfaction?

### INSIGHT #8: Churned Customers Experience More Dropped Calls
Churned customers have a much higher average call drop rate compared to active customers.

- Active customers drop rate: **5%**
- Churned customers drop rate: **30%**

This strongly suggests that poor network quality contributes to customer churn.

### INSIGHT #9: Some Towers Have Poor Network Performance
Towers such as:

- TOWER_019
- TOWER_006
- TOWER_031

show the highest dropped call percentages.

These towers may require maintenance or network optimization.

---

## KPI 5: Which Plans, Regions, or Segments Generate Revenue or Risk?

### INSIGHT #10: Prepaid Segment Generates the Highest Revenue
The Prepaid segment generates the highest total revenue among all customer segments.

However, it also has the highest churn risk.

This creates a major business challenge:

- High revenue
- But unstable customer retention

### INSIGHT #11: Major Cities Drive Most Revenue
Cities like:

- Mansoura
- Tanta
- Aswan
- Luxor

generate the highest telecom revenue.

These cities are critical business markets for the company.

### INSIGHT #12: PLAN_A Is the Highest-Risk Plan
PLAN_A has the highest churn rate at around **42.7%**.

This may indicate:

- Pricing problems
- Weak customer satisfaction
- Poor perceived value

### INSIGHT #13: Business Customers Are the Most Stable Segment
Business customers have the lowest churn rate and more stable subscription behavior.

This makes them one of the most valuable customer groups for long-term profitability.

---

# Final Business Conclusion

The analysis shows that customer churn is mainly influenced by:

- High monthly charges
- Payment failures
- Dropped calls and network quality
- Customer segment type
- Plan selection

### Recommended Actions

- Improve prepaid customer retention
- Reduce dropped calls and network issues
- Monitor failed payments proactively
- Optimize high-risk plans like PLAN_A
- Improve customer experience in high-churn cities

---

##  How to Run

1. Load source CSV files into staging tables
2. Execute ETL pipelines in Informatica Cloud:

   * STG mappings
   * HUB mappings
   * LINK mappings
   * SAT mappings
3. Validate data in SQL Server
4. Connect Power BI to Data Mart

---

##  Project Highlights

* Designed enterprise-grade data warehouse architecture
* Implemented Data Vault best practices (hash keys, hashdiff, insert-only)
* Built scalable pipelines handling multiple telecom datasets
* Delivered analytical insights for business decision-making

---


---
