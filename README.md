# SQL_E-Commerce_Dataset


SELECT
        FORMAT_DATE("%Y%m", PARSE_DATE("%Y%m%d",date)) month
        ,SUM(totals.visits) visits
        ,SUM(totals.pageviews) pageviews
        ,SUM(totals.transactions) transactions
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
where _table_suffix between '0101' and '0331'
GROUP BY 1
ORDER BY 1 ;
