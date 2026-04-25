# Retail_Data_Quallity__Audit
# Retail Data Quality Audit & Reporting Project

## Project Overview
This project demonstrates the end-to-end process of **Data Auditing, Cleansing, and Business Reporting** using SQL. Working with a retail sales dataset, I identified critical data integrity issues that would lead to inaccurate financial reporting and resolved them to ensure a "Single Source of Truth."

##  Skills & Tools Used
* **SQL (SQLite):** Data extraction, filtering, and manipulation.
* **Data Quality Assurance:** Identifying duplicates, null values, and logic errors.
* **Business Intelligence:** Creating summary reports for management decision-making.
* **Troubleshooting:** Resolving schema import errors and syntax debugging.

##  Phase 1: The Data Audit 
Before using the data, I ran a series of "Integrity Checks" to find errors.

### 1.The Math Integrity Check
I checked if the `Price Per Unit * Quantity` actually matched the `Total Spent`. 
**The Why:** If these don't match, the company’s revenue reports are wrong.
```sql
SELECT "Transaction ID", "Item", "Price Per Unit", "Quantity", "Total Spent"
FROM retail_store_sales
WHERE ("Price Per Unit" * "Quantity") != "Total Spent";
```

### 2.The Duplicate Hunt
I checked for duplicate Transaction IDs to ensure no sale was counted twice.
**The Why:** Duplicates inflate sales figures and ruin inventory counts.
```sql
SELECT "Transaction ID", COUNT(*)
FROM retail_store_sales
GROUP BY "Transaction ID"
HAVING COUNT(*) > 1;
```

---

## Phase 2:Data Cleansing (The Fix)
Once errors were identified, I used the `UPDATE` command to fix the records.

**The Logic:** I recalculated the `Total Spent` column based on the verified `Price` and `Quantity` variables to ensure the math was 100% accurate.
```sql
UPDATE retail_store_sales
SET "Total Spent" = "Price Per Unit" * "Quantity"
WHERE ("Price Per Unit" * "Quantity") != "Total Spent";
```
*I then verified this by re-running the audit query, which returned **0 errors**.*
---

## Phase 3: Business Insights (The Report)
With clean data, I generated a **Top 5 VIP Customer Report**. 

**The Purpose:** To help the marketing team identify high-value clients for loyalty rewards.
```sql
SELECT "Customer ID", SUM("Total Spent") AS total_customer_value
FROM retail_store_sales
GROUP BY "Customer ID"
ORDER BY total_customer_value DESC
LIMIT 5;
```

---
## Key Learnings
* **Data Validation:** Learned that "Dirty Data" is common and an Analyst's first job is to verify accuracy.
* **Technical Precision:** Mastered the use of double quotes for columns with spaces and the difference between `WHERE` and `HAVING`.
* **Remote Readiness:** Practiced documenting my workflow so that my results are transparent and reproducible by a remote team.
```
