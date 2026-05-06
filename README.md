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

## Author

**Khalid Mohamed**


---
