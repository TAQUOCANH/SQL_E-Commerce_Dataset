# SQL_E-Commerce_Dataset
### Query 01: calculate total visit, pageview, transaction for Jan, Feb and March 2017 (order by month)

```sql
SELECT
        FORMAT_DATE("%Y%m", PARSE_DATE("%Y%m%d",date)) month
        ,SUM(totals.visits) visits
        ,SUM(totals.pageviews) pageviews
        ,SUM(totals.transactions) transactions
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
where _table_suffix between '0101' and '0331'
GROUP BY 1
ORDER BY 1 ;
```

| month  | visits | pageviews | transactions |
|--------|--------|-----------|--------------|
| 201701 | 64694  | 257708    | 713          |
| 201702 | 62192  | 233373    | 733          |
| 201703 | 69931  | 259522    | 993          |

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


| source                           | total_visits | total_no_of_bounces | bounce_rate |
|----------------------------------|--------------|---------------------|-------------|
| google                           | 38400        | 19798               | 51.56       |
| (direct)                         | 19891        | 8606                | 43.27       |
| youtube.com                      | 6351         | 4238                | 66.73       |
| analytics.google.com             | 1972         | 1064                | 53.96       |
| Partners                         | 1788         | 936                 | 52.35       |
| m.facebook.com                   | 669          | 430                 | 64.28       |
| google.com                       | 368          | 183                 | 49.73       |
| dfa                              | 302          | 124                 | 41.06       |
| sites.google.com                 | 230          | 97                  | 42.17       |
| facebook.com                     | 191          | 102                 | 53.4        |
| reddit.com                       | 189          | 54                  | 28.57       |
| qiita.com                        | 146          | 72                  | 49.32       |
| quora.com                        | 140          | 70                  | 50.0        |
| baidu                            | 140          | 84                  | 60.0        |
| bing                             | 111          | 54                  | 48.65       |
| mail.google.com                  | 101          | 25                  | 24.75       |
| yahoo                            | 100          | 41                  | 41.0        |
| blog.golang.org                  | 65           | 19                  | 29.23       |
| l.facebook.com                   | 51           | 45                  | 88.24       |
| groups.google.com                | 50           | 22                  | 44.0        |
| t.co                             | 38           | 27                  | 71.05       |
| google.co.jp                     | 36           | 25                  | 69.44       |
| m.youtube.com                    | 34           | 22                  | 64.71       |
| dealspotr.com                    | 26           | 12                  | 46.15       |
| productforums.google.com         | 25           | 21                  | 84.0        |
| ask                              | 24           | 16                  | 66.67       |
| support.google.com               | 24           | 16                  | 66.67       |
| int.search.tb.ask.com            | 23           | 17                  | 73.91       |
| optimize.google.com              | 21           | 10                  | 47.62       |
| docs.google.com                  | 20           | 8                   | 40.0        |
| lm.facebook.com                  | 18           | 9                   | 50.0        |
| l.messenger.com                  | 17           | 6                   | 35.29       |
| duckduckgo.com                   | 16           | 14                  | 87.5        |
| adwords.google.com               | 16           | 7                   | 43.75       |
| google.co.uk                     | 15           | 7                   | 46.67       |
| sashihara.jp                     | 14           | 8                   | 57.14       |
| lunametrics.com                  | 13           | 8                   | 61.54       |
| search.mysearch.com              | 12           | 11                  | 91.67       |
| outlook.live.com                 | 10           | 7                   | 70.0        |
| tw.search.yahoo.com              | 10           | 8                   | 80.0        |
| phandroid.com                    | 9            | 7                   | 77.78       |
| connect.googleforwork.com        | 8            | 5                   | 62.5        |
| plus.google.com                  | 8            | 2                   | 25.0        |
| m.yz.sm.cn                       | 7            | 5                   | 71.43       |
| google.co.in                     | 6            | 3                   | 50.0        |
| search.xfinity.com               | 6            | 6                   | 100.0       |
| online-metrics.com               | 5            | 2                   | 40.0        |
| hangouts.google.com              | 5            | 1                   | 20.0        |
| s0.2mdn.net                      | 5            | 3                   | 60.0        |
| google.ru                        | 5            | 1                   | 20.0        |
| in.search.yahoo.com              | 4            | 2                   | 50.0        |
| googleads.g.doubleclick.net      | 4            | 1                   | 25.0        |
| away.vk.com                      | 4            | 3                   | 75.0        |
| m.sogou.com                      | 4            | 3                   | 75.0        |
| m.baidu.com                      | 3            | 2                   | 66.67       |
| siliconvalley.about.com          | 3            | 2                   | 66.67       |
| getpocket.com                    | 3            |                     |             |
| centrum.cz                       | 2            | 2                   | 100.0       |
| github.com                       | 2            | 2                   | 100.0       |
| plus.url.google.com              | 2            |                     |             |
| myactivity.google.com            | 2            | 1                   | 50.0        |
| uk.search.yahoo.com              | 2            | 1                   | 50.0        |
| search.1and1.com                 | 2            | 2                   | 100.0       |
| google.cl                        | 2            | 1                   | 50.0        |
| moodle.aurora.edu                | 2            | 2                   | 100.0       |
| m.sp.sm.cn                       | 2            | 2                   | 100.0       |
| amp.reddit.com                   | 2            | 1                   | 50.0        |
| au.search.yahoo.com              | 2            | 2                   | 100.0       |
| google.it                        | 2            | 1                   | 50.0        |
| google.co.th                     | 2            | 1                   | 50.0        |
| wap.sogou.com                    | 2            | 2                   | 100.0       |
| msn.com                          | 2            | 1                   | 50.0        |
| calendar.google.com              | 2            | 1                   | 50.0        |
| arstechnica.com                  | 1            |                     |             |
| ph.search.yahoo.com              | 1            |                     |             |
| web.mail.comcast.net             | 1            | 1                   | 100.0       |
| es.search.yahoo.com              | 1            | 1                   | 100.0       |
| images.google.com.au             | 1            | 1                   | 100.0       |
| online.fullsail.edu              | 1            | 1                   | 100.0       |
| mx.search.yahoo.com              | 1            | 1                   | 100.0       |
| search.tb.ask.com                | 1            |                     |             |
| it.pinterest.com                 | 1            | 1                   | 100.0       |
| google.bg                        | 1            | 1                   | 100.0       |
| news.ycombinator.com             | 1            | 1                   | 100.0       |
| web.facebook.com                 | 1            | 1                   | 100.0       |
| suche.t-online.de                | 1            | 1                   | 100.0       |
| google.com.br                    | 1            |                     |             |
| google.nl                        | 1            |                     |             |
| kik.com                          | 1            | 1                   | 100.0       |
| gophergala.com                   | 1            | 1                   | 100.0       |
| kidrex.org                       | 1            | 1                   | 100.0       |
| newclasses.nyu.edu               | 1            |                     |             |
| earth.google.com                 | 1            |                     |             |
| malaysia.search.yahoo.com        | 1            | 1                   | 100.0       |
| aol                              | 1            |                     |             |
| google.es                        | 1            | 1                   | 100.0       |
| google.ca                        | 1            |                     |             |

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

<p>This table provides an overview of the revenue origins from different channels over periodic time frames. Revenue is primarily driven by direct sources and Google, with smaller but still significant contributions from other sources.</p>

### Query 04: Average number of pageviews by purchaser type (purchasers vs non-purchasers) in June, July 2017.

```sql
WITH
avg_pageviews_purchase AS(
  SELECT
        FORMAT_DATE("%Y%m", PARSE_DATE("%Y%m%d",date)) month
        ,SUM(totals.pageviews) /  COUNT(DISTINCT fullVisitorId) avg_pageviews_purchase
  FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*` ,
  UNNEST (hits) hits,
  UNNEST (hits.product) product
  where _table_suffix between '0601' and '0731' and totals.transactions >=1 and product.productRevenue IS NOT NULL
  GROUP BY 1
)
, avg_pageviews_non_purchase AS (
    SELECT
        FORMAT_DATE("%Y%m", PARSE_DATE("%Y%m%d",date)) month
        ,SUM(totals.pageviews) /  COUNT(DISTINCT fullVisitorId) avg_pageviews_non_purchase
  FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*` ,
  UNNEST (hits) hits,
  UNNEST (hits.product) product
  where _table_suffix between '0601' and '0731' and totals.transactions IS NULL and product.productRevenue IS NULL
  GROUP BY 1
)

SELECT * FROM avg_pageviews_purchase
FULL JOIN avg_pageviews_non_purchase
USING(month)
ORDER BY month ;

```

| Month  | Avg Pageviews Purchase | Avg Pageviews Non-Purchase |
|--------|-------------------------|----------------------------|
| 201706 | 94.02050113895217       | 316.86558846341671         |
| 201707 | 124.23755186721992      | 334.05655979568053         |


<p> The average pageviews for purchases increased from June to July, indicating that users who made purchases viewed more pages on average in July</p>
<p>The average pageviews for non-purchases also increased from June to July, suggesting that users who did not make purchases were viewing more pages as well</p>

### Query 05: Average number of transactions per user that made a purchase in July 2017

```sql
SELECT
        FORMAT_DATE("%Y%m", PARSE_DATE("%Y%m%d",date)) month
        ,SUM(totals.transactions) /  COUNT(DISTINCT fullVisitorId) avg_total_transactions_per_user
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*` ,
UNNEST (hits) hits,
UNNEST (hits.product) product
where _table_suffix between '0701' and '0731' and totals.transactions >=1 and product.productRevenue IS NOT NULL
GROUP BY 1;
```

| Month  | Avg Total Transactions per User |
|--------|---------------------------------|
| 201707 | 4.16390041493776                |


<p>In July 2017, each user made an average of approximately 4.16 transactions</p>

### Query 06: Average amount of money spent per session. Only include purchaser data in July 2017

```sql
SELECT
        FORMAT_DATE("%Y%m", PARSE_DATE("%Y%m%d",date)) month
        ,SUM(product.productRevenue) /  (COUNT (visitId) *1000000.0) avg_total_transactions_per_user
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*` ,
UNNEST (hits) hits,
UNNEST (hits.product) product
where _table_suffix between '0701' and '0731' and totals.transactions >=1 and product.productRevenue IS NOT NULL
GROUP BY 1 ;
```

| Month  | Avg Total Transactions per User |
|--------|---------------------------------|
| 201707 | 43.856598348051243              |


<p>In July 2017, each user made an average of approximately 43.86 transactions, indicating a very high level of purchasing activity per user during this month</p> 
