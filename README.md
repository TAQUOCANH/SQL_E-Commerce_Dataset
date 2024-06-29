# SQL_E-Commerce_Dataset
### Query 01: calculate total visit, pageview, transaction for Jan, Feb and March 2017 (order by month)

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
<p>March 2017 shows a significant improvement in all metrics (visits, pageviews, and transactions) compared to January and February </p>

### Query 02: Bounce rate per traffic source in July 2017 (Bounce_rate = num_bounce/total_visit) (order by total_visit DESC)
```sql
SELECT
      trafficSource.source
      ,SUM(totals.visits) total_visits
      ,SUM(totals.bounces) total_no_of_bounces
      ,ROUND(SUM(totals.bounces) * 100.00 / SUM(totals.visits), 2)  bounce_rate
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`
GROUP BY 1
ORDER BY 2 DESC ;
```
<img width="800" alt="Screenshot_7" src="https://github.com/TAQUOCANH/SQL_E-Commerce_Dataset/assets/135592751/30f98362-1370-4cdd-965d-a9b0e9656ef6">
<p>The table analyzes the sources of website traffic, including total visits, total number of bounces, and bounce rate from various sources.</p>
<p>Optimize marketing strategies and improve user experience to reduce bounce rates, especially from sources with high bounce rates like Google and YouTube.</p>

### Query 3: Revenue by traffic source by week, by month in June 2017
```sql
WITH
week_rev AS (
  SELECT
        "Week" time_type
        ,FORMAT_DATE("%Y%W", PARSE_DATE("%Y%m%d",date)) time
        ,trafficSource.source
        ,SUM(product.productRevenue)/ 1000000.00 revenue
  FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201706*` ,
  UNNEST (hits) hits,
  UNNEST (hits.product) product
  WHERE product.productRevenue IS NOT NULL
  GROUP BY 1,2,3
)
,month_rev AS (
  SELECT
        "Month" time_type
        ,FORMAT_DATE("%Y%m", PARSE_DATE("%Y%m%d",date)) time
        ,trafficSource.source
        ,SUM(product.productRevenue)/ 1000000.00 revenue
  FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201706*` ,
  UNNEST (hits) hits,
  UNNEST (hits.product) product
  WHERE product.productRevenue IS NOT NULL
  GROUP BY 1,2,3
)

SELECT *
FROM month_rev
UNION ALL
SELECT *
FROM week_rev
ORDER BY revenue DESC,source, time_type, time ;
```

Dưới đây là dữ liệu đã được sắp xếp thành bảng:

| time_type | time   | source                | revenue       |
|-----------|--------|-----------------------|---------------|
| Month     | 201706 | (direct)              | 97333.619695  |
| Week      | 201724 | (direct)              | 30908.909927  |
| Week      | 201725 | (direct)              | 27295.319924  |
| Month     | 201706 | google                | 18757.17992   |
| Week      | 201723 | (direct)              | 17325.679919  |
| Week      | 201726 | (direct)              | 14914.80995   |
| Week      | 201724 | google                | 9217.169976   |
| Month     | 201706 | dfa                   | 8862.229996   |
| Week      | 201722 | (direct)              | 6888.899975   |
| Week      | 201726 | google                | 5330.569964   |
| Week      | 201726 | dfa                   | 3704.74       |
| Month     | 201706 | mail.google.com       | 2563.13       |
| Week      | 201724 | mail.google.com       | 2486.86       |
| Week      | 201724 | dfa                   | 2341.56       |
| Week      | 201722 | google                | 2119.38999    |
| Week      | 201722 | dfa                   | 1670.649998   |
| Week      | 201723 | dfa                   | 1145.279998   |
| Week      | 201723 | google                | 1083.949999   |
| Week      | 201725 | google                | 1006.099991   |
| Month     | 201706 | search.myway.com      | 105.939998    |
| Week      | 201723 | search.myway.com      | 105.939998    |
| Month     | 201706 | groups.google.com     | 101.96        |
| Week      | 201725 | mail.google.com       | 76.27         |
| Month     | 201706 | chat.google.com       | 74.03         |
| Week      | 201723 | chat.google.com       | 74.03         |
| Month     | 201706 | dealspotr.com         | 72.95         |
| Week      | 201724 | dealspotr.com         | 72.95         |
| Month     | 201706 | mail.aol.com          | 64.849998     |
| Week      | 201725 | mail.aol.com          | 64.849998     |
| Week      | 201726 | groups.google.com     | 63.37         |
| Month     | 201706 | phandroid.com         | 52.95         |
| Week      | 201725 | phandroid.com         | 52.95         |
| Month     | 201706 | sites.google.com      | 39.17         |
| Week      | 201725 | groups.google.com     | 38.59         |
| Week      | 201725 | sites.google.com      | 25.19         |
| Month     | 201706 | google.com            | 23.99         |
| Week      | 201725 | google.com            | 23.99         |
| Month     | 201706 | yahoo                 | 20.39         |
| Week      | 201726 | yahoo                 | 20.39         |
| Month     | 201706 | youtube.com           | 16.99         |
| Week      | 201723 | youtube.com           | 16.99         |
| Month     | 201706 | bing                  | 13.98         |
| Week      | 201724 | bing                  | 13.98         |
| Week      | 201722 | sites.google.com      | 13.98         |
| Month     | 201706 | l.facebook.com        | 12.48         |
| Week      | 201724 | l.facebook.com        | 12.48         |
