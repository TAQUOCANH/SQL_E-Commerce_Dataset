# SQL_E-Commerce_Dataset

## DATASET

| Field Name                      | Data Type | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|---------------------------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| fullVisitorId                   | STRING    | The unique visitor ID.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| date                            | STRING    | The date of the session in YYYYMMDD format.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| totals                          | RECORD    | This section contains aggregate values across the session.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| totals.bounces                  | INTEGER   | Total bounces (for convenience). For a bounced session, the value is 1, otherwise it is null.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| totals.hits                     | INTEGER   | Total number of hits within the session.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| totals.pageviews                | INTEGER   | Total number of pageviews within the session.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| totals.visits                   | INTEGER   | The number of sessions (for convenience). This value is 1 for sessions with interaction events. The value is null if there are no interaction events in the session.                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| totals.transactions             | INTEGER   | Total number of ecommerce transactions within the session.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| trafficSource.source            | STRING    | The source of the traffic source. Could be the name of the search engine, the referring hostname, or a value of the utm_source URL parameter.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| hits                            | RECORD    | This row and nested fields are populated for any and all types of hits.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| hits.eCommerceAction            | RECORD    | This section contains all of the ecommerce hits that occurred during the session. This is a repeated field and has an entry for each hit that was collected.                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| hits.eCommerceAction.action_type| STRING    | The action type. Click through of product lists = 1, Product detail views = 2, Add product(s) to cart = 3, Remove product(s) from cart = 4, Check out = 5, Completed purchase = 6, Refund of purchase = 7, Checkout options = 8, Unknown = 0. Usually this action type applies to all the products in a hit, with the following exception: when hits.product.isImpression = TRUE, the corresponding product is a product impression that is seen while the product action is taking place (i.e., a "product in list view"). Example query to calculate number of products in list views: SELECT COUNT(hits.product.v2ProductName) FROM [foo-160803:123456789.ga_sessions_20170101] WHERE hits.product.isImpression == TRUE Example query to calculate number of products in detailed view: SELECT COUNT(hits.product.v2ProductName), FROM [foo-160803:123456789.ga_sessions_20170101] WHERE hits.ecommerceaction.action_type = '2' AND ( BOOLEAN(hits.product.isImpression) IS NULL OR BOOLEAN(hits.product.isImpression) == FALSE )|
| hits.product                    | RECORD    | This row and nested fields will be populated for each hit that contains Enhanced Ecommerce PRODUCT data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| hits.product.productQuantity    | INTEGER   | The quantity of the product purchased.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| hits.product.productRevenue     | INTEGER   | The revenue of the product, expressed as the value passed to Analytics multiplied by 10^6 (e.g., 2.40 would be given as 2400000).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| hits.product.productSKU         | STRING    | Product SKU.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| hits.product.v2ProductName      | STRING    | Product Name.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| fullVisitorId                   | STRING    | The unique visitor ID.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |


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

| month  | avg_pageviews_purchase | avg_pageviews_non_purchase |
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

| month  | avg_total_transactions_per_user |
|--------|---------------------------------|
| 201707 | 4.16390041493776                |


<p>In July 2017, each user made an average of approximately 4.16 transactions</p>

### Query 06: Average amount of money spent per session. Only include purchaser data in July 2017

```sql
SELECT
        FORMAT_DATE("%Y%m", PARSE_DATE("%Y%m%d",date)) month
        ,SUM(product.productRevenue) /  (COUNT (visitId) *1000000.0) avg_revenue_by_user_per_visit
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*` ,
UNNEST (hits) hits,
UNNEST (hits.product) product
where _table_suffix between '0701' and '0731' and totals.transactions >=1 and product.productRevenue IS NOT NULL
GROUP BY 1 ;
```

| month  | avg_revenue_by_user_per_visit |
|--------|---------------------------------|
| 201707 | 43.856598348051243              |


<p>July 2017 had an average revenue per user per visit of 43.86.</p> 
<p>This insight can help the business evaluate and optimize marketing strategies to enhance engagement and conversion rates, turning visits into revenue more effectively.</p>

### Query 07: Other products purchased by customers who purchased product "YouTube Men's Vintage Henley" in July 2017. Output should show product name and the quantity was ordered.

```sql
SELECT 
    product.v2ProductName AS other_purchased_products,
    SUM(product.productQuantity) AS quantity
FROM 
    `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
    UNNEST(hits) AS hits,
    UNNEST(hits.product) AS product
WHERE 
    _table_suffix BETWEEN '0701' AND '0731'
    AND product.productRevenue IS NOT NULL
    AND product.v2ProductName != "YouTube Men's Vintage Henley"
    AND fullVisitorId IN (
        SELECT DISTINCT fullVisitorId
        FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
        UNNEST(hits) AS hits,
        UNNEST(hits.product) AS product
        WHERE 
            _table_suffix BETWEEN '0701' AND '0731'
            AND product.v2ProductName = "YouTube Men's Vintage Henley"
            AND product.productRevenue IS NOT NULL
    )
GROUP BY 1
ORDER BY 2 DESC ;
```
### Table

| other_purchased_products                           | quantity |
|----------------------------------------------------|----------|
| Google Sunglasses                                  | 20       |
| Google Women's Vintage Hero Tee Black              | 7        |
| SPF-15 Slim & Slender Lip Balm                     | 6        |
| Google Women's Short Sleeve Hero Tee Red Heather   | 4        |
| YouTube Men's Fleece Hoodie Black                  | 3        |
| Google Men's Short Sleeve Badge Tee Charcoal       | 3        |
| 22 oz YouTube Bottle Infuser                       | 2        |
| Android Men's Vintage Henley                       | 2        |
| YouTube Twill Cap                                  | 2        |
| Google Men's Short Sleeve Hero Tee Charcoal        | 2        |
| Red Shine 15 oz Mug                                | 2        |
| Google Doodle Decal                                | 2        |
| Recycled Mouse Pad                                 | 2        |
| Android Women's Fleece Hoodie                      | 2        |
| Android Wool Heather Cap Heather/Black             | 2        |
| Crunch Noise Dog Toy                               | 2        |
| Google Men's Vintage Badge Tee White               | 1        |
| Google Slim Utility Travel Bag                     | 1        |
| YouTube Women's Short Sleeve Hero Tee Charcoal     | 1        |
| Google Men's Performance Full Zip Jacket Black     | 1        |
| 26 oz Double Wall Insulated Bottle                 | 1        |
| Android Men's Short Sleeve Hero Tee White          | 1        |
| Android Men's Pep Rally Short Sleeve Tee Navy      | 1        |
| YouTube Men's Short Sleeve Hero Tee Black          | 1        |
| Google Men's Pullover Hoodie Grey                  | 1        |
| YouTube Men's Short Sleeve Hero Tee White          | 1        |
| Google Men's 100% Cotton Short Sleeve Hero Tee Red | 1        |
| Android Men's Vintage Tank                         | 1        |
| Google Men's Zip Hoodie                            | 1        |
| Google Twill Cap                                   | 1        |
| Google Men's Long & Lean Tee Grey                  | 1        |
| Google Men's Long Sleeve Raglan Ocean Blue         | 1        |
| YouTube Custom Decals                              | 1        |
| Four Color Retractable Pen                         | 1        |
| Google Laptop and Cell Phone Stickers              | 1        |
| Google Men's Vintage Badge Tee Black               | 1        |
| Google Men's Long & Lean Tee Charcoal              | 1        |
| Google Men's Bike Short Sleeve Tee Charcoal        | 1        |
| Google 5-Panel Cap                                 | 1        |
| Google Toddler Short Sleeve T-shirt Grey           | 1        |
| Android Sticker Sheet Ultra Removable              | 1        |
| Google Men's Performance 1/4 Zip Pullover Heather/Black | 1   |
| YouTube Hard Cover Journal                         | 1        |
| Android BTTF Moonshot Graphic Tee                  | 1        |
| Google Men's Airflow 1/4 Zip Pullover Black        | 1        |
| Android Men's Short Sleeve Hero Tee Heather        | 1        |
| YouTube Women's Short Sleeve Tri-blend Badge Tee Charcoal | 1  |
| YouTube Men's Long & Lean Tee Charcoal             | 1        |
| Google Women's Long Sleeve Tee Lavender            | 1        |
| 8 pc Android Sticker Sheet                         | 1        |


<p>Google Sunglasses was the most frequently purchased product, with a quantity of 20.</p> 
<p>Google Women's Vintage Hero Tee Black and SPF-15 Slim & Slender Lip Balm were also popular, with quantities of 7 and 6 respectively.</p>
<p>Various other products were purchased in smaller quantities, ranging from 4 to 1 units.</p>
<p>This suggests that customers who bought the YouTube Men's Vintage Henley in July 2017 also showed a diverse interest in other Google and YouTube merchandise, indicating potential cross-selling opportunities.</p>

### Query 08: Calculate cohort map from product view to addtocart to purchase in Jan, Feb and March 2017. For example, 100% product view then 40% add_to_cart and 10% purchase. Add_to_cart_rate = number product  add to cart/number product view. Purchase_rate = number product purchase/number product view. The output should be calculated in product level.
```sql
WITH product_data AS(
SELECT
        FORMAT_DATE("%Y%m", PARSE_DATE("%Y%m%d",date)) month
        ,COUNT(CASE WHEN eCommerceAction.action_type = '2' THEN product.v2ProductName END) num_product_view
        ,COUNT(CASE WHEN eCommerceAction.action_type = '3' THEN product.v2ProductName END) num_add_to_cart
        ,COUNT(CASE WHEN eCommerceAction.action_type = '6' AND product.productRevenue IS NOT NULL THEN product.v2ProductName END) num_purchase
        
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*` ,
UNNEST (hits) hits,
UNNEST (hits.product) product
where _table_suffix between '0101' and '1231' AND eCommerceAction.action_type in ('2','3','6')
GROUP BY 1
ORDER BY 1
)

SELECT
    *,
    ROUND(num_add_to_cart/num_product_view * 100, 2)  add_to_cart_rate,
    ROUND(num_purchase/num_product_view * 100, 2)  purchase_rate
FROM product_data;
```

| month  | num_product_view | num_add_to_cart | num_purchase | add_to_cart_rate | purchase_rate |
|--------|-------------------|-----------------|--------------|------------------|---------------|
| 201701 | 25787             | 7342            | 2143         | 28.47            | 8.31          |
| 201702 | 21489             | 7360            | 2060         | 34.25            | 9.59          |
| 201703 | 23549             | 8782            | 2977         | 37.29            | 12.64         |
| 201704 | 24587             | 10291           | 2906         | 41.86            | 11.82         |
| 201705 | 25469             | 10083           | 3285         | 39.59            | 12.90         |
| 201706 | 22148             | 9020            | 2785         | 40.73            | 12.57         |
| 201707 | 28576             | 11860           | 3669         | 41.50            | 12.84         |
| 201708 | 1267              | 494             | 186          | 38.99            | 14.68         |

<p> Overall, the data without August 2017 suggests a positive trend in user engagement and conversion rates, with significant increases in product views, items added to the cart, and purchases from January to July 2017. The exclusion of the incomplete data for August ensures the integrity and accuracy of these insights </p>
