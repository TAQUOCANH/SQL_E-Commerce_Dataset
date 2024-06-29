# SQL_E-Commerce_Dataset
Query 01: calculate total visit, pageview, transaction for Jan, Feb and March 2017 (order by month)

```sql
   SELECT 
    FORMAT_DATE("%Y%m",PARSE_DATE("%Y%m%d",date)) month_extract
    ,SUM(totals.visits) visits
    ,SUM(totals.pageviews) pageviews
    ,SUM(totals.transactions) transactions
    ,ROUND(SUM(totals.totalTransactionRevenue)/POW(10,6),2) revenue
   FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
   WHERE _table_suffix BETWEEN '0101' AND '0331'
   GROUP BY month_extract
```
<img width="800" alt="query 1" src="https://github.com/TAQUOCANH/SQL_E-Commerce_Dataset/assets/135592751/5b39e16a-9ac3-48cd-88a0-1748a4bdbcf0">
