
### 2025-02-06 22:17:21 [GCP Data Engineer Mock interview](https://www.youtube.com/watch?v=qZwffdeu1sY)
reference:xxxxx
xxxxx
_______________________________________________________________
### 2025-02-06 22:07:45 Could you please explain the high-level architecture of BigQuery?
BigQuery is a data warehousing tool designed for analytical purposes. It has a ***separation of storage and compute***, meaning they operate independently. BigQuery uses ***columnar storage*** (OLAP) instead of traditional row-based (OLTP) storage. This enables efficient querying because it ***scans only the specific columns needed***, improving performance.
_______________________________________________________________
### 2025-02-06 22:12:05 Do i need to add index in big query table to get a faster query speed?
No, you do not need to add traditional indexes in BigQuery. BigQuery is a columnar, distributed data warehouse that automatically optimizes query performance through its architecture. It handles data storage and retrieval differently from traditional relational databases.

For Faster Query Performance:
Partitioning: Use partitioned tables to reduce the amount of data scanned during queries.
Clustering: Apply clustering on frequently filtered or joined columns to improve performance.
Denormalization: Flatten your data where possible to reduce complex joins.
Materialized Views: Use materialized views for pre-aggregated data.
Query Optimization: Optimize SQL queries with proper filtering, avoiding SELECT *, and using approximate aggregation functions like APPROX_COUNT_DISTINCT.
_______________________________________________________________
### 2025-02-06 22:13:29 why we have both where and having in sql
reference: https://www.youtube.com/watch?v=BHwzDmr6d7s&t=1s

*WHERE Clause:*

Purpose: Filters rows before any grouping or aggregation occurs.
Used with: Regular columns (not aggregate functions like COUNT(), SUM(), etc.).

*HAVING Clause:*

Purpose: Filters groups after aggregation (GROUP BY) has been applied.
Used with: Aggregate functions (COUNT(), SUM(), AVG(), etc.).

SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 10;
_______________________________________________________________
### 2025-02-06 22:16:06 how many ways to Load Data from GCS to BigQuery
reference:[https://medium.com/@santosh_beora/loading-data-from-gcs-to-bigquery-a-comprehensive-guide-62b5d3abea53](https://medium.com/@santosh_beora/loading-data-from-gcs-to-bigquery-a-comprehensive-guide-62b5d3abea53)
xxxxx
_______________________________________________________________
### 2025-02-06 22:19:54 How can we optimize performance in BigQuery?
reference: xxxxx

Partitioning: Organize large tables by date or other logical partitions to reduce the amount of data scanned.
Clustering: Group related data together to improve query performance when filtering by clustered columns.
Query Optimization: Use filters (WHERE conditions), avoid SELECT *, and limit data processed.
_______________________________________________________________
### 2025-02-06 22:20:46 In terms of cost optimization, how can we save costs in BigQuery?
reference: [Cost optimization best practices for BigQuery](https://www.youtube.com/watch?v=cZgTavxWO2k)

Reserved Slots: Purchase slots in advance for predictable workloads to reduce costs.
Optimize Queries: Use partitioning and clustering to reduce data scanned.
Storage Optimization: Move rarely accessed data to cheaper storage like Cloud Storage (Nearline/Coldline).
_______________________________________________________________
### 2025-02-06 23:08:11 For example, if we have thousands of datasets and tables that people are not using anymore, how can we identify the unused datasets and tables? What would be your action plan and point of view to save costs in such cases?
reference: xxxxx

1. Identify Unused Tables:
- Use BigQuery Information Schema to check the last access times.
- Analyze Cloud Audit Logs for usage patterns.
2. Action Plan:
- Archive unused tables to Cloud Storage (Nearline or Coldline) to reduce costs.
- Regularly review and clean up unused datasets.
_______________________________________________________________