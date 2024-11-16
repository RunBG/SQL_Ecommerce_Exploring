# SQL_Ecommerce_Exploring

## 1. Introduction and Motivation
The eCommerce dataset is publicly available on Google BigQuery and contains information about user sessions on a website collected from Google Analytics in 2017.

The author utilizes this dataset to perform queries analyzing website activity during 2017. These analyses include calculating bounce rates, identifying days with the highest revenue, examining user behavior on pages, and conducting various other types of assessments. The project aims to provide insights into the business performance, marketing efficiency, and product analysis.

To query and work with this dataset, the author uses the Google BigQuery tool to write and execute SQL queries.
## 2. The goal of creating this project
- Overview of website activity
- Product analysis
- Revenue analysis
- Transactions analysis
- Type purchaser analysis
- Bounce rate analysis
## 3. Read and explain dataset
| Field Name                           | Data Type  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|--------------------------------------|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| fullVisitorId                        | STRING     | The unique visitor ID\.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| date                                 | STRING     | The date of the session in YYYYMMDD format\.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| totals                               | RECORD     | This section contains aggregate values across the session\.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| totals\.bounces                      | INTEGER    | Total bounces \(for convenience\)\. For a bounced session, the value is 1, otherwise it is null\.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| totals\.hits                         | INTEGER    | Total number of hits within the session\.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| totals\.pageviews                    | INTEGER    | Total number of pageviews within the session\.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| totals\.visits                       | INTEGER    | The number of sessions \(for convenience\)\. This value is 1 for sessions with interaction events\. The value is null if there are no interaction events in the session\.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| totals\.transactions                 | INTEGER    | Total number of ecommerce transactions within the session\.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| trafficSource\.source                | STRING     | The source of the traffic source\. Could be the name of the search engine, the referring hostname, or a value of the utm\_source URL parameter\.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| hits                                 | RECORD     | This row and nested fields are populated for any and all types of hits\.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| hits\.eCommerceAction                | RECORD     | This section contains all of the ecommerce hits that occurred during the session\. This is a repeated field and has an entry for each hit that was collected\.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| hits\.eCommerceAction\.action\_type  | STRING     | "The action type\. Click through of product lists = 1, Product detail views = 2, Add product\(s\) to cart = 3, Remove product\(s\) from cart = 4, Check out = 5, Completed purchase = 6, Refund of purchase = 7, Checkout options = 8, Unknown = 0\.<br>Usually this action type applies to all the products in a hit, with the following exception: when hits\.product\.isImpression = TRUE, the corresponding product is a product impression that is seen while the product action is taking place \(i\.e\., a ""product in list view""\)\.<br>Example query to calculate number of products in list views:<br>SELECT<br>COUNT\(hits\.product\.v2ProductName\)<br>FROM \[foo\-160803:123456789\.ga\_sessions\_20170101\]<br>WHERE hits\.product\.isImpression == TRUE<br>Example query to calculate number of products in detailed view:<br>SELECT<br>COUNT\(hits\.product\.v2ProductName\),<br>FROM<br>\[foo\-160803:123456789\.ga\_sessions\_20170101\]<br>WHERE<br>hits\.ecommerceaction\.action\_type = '2'<br>AND \( BOOLEAN\(hits\.product\.isImpression\) IS NULL OR BOOLEAN\(hits\.product\.isImpression\) == FALSE \)" |
| hits\.product                        | RECORD     | This row and nested fields will be populated for each hit that contains Enhanced Ecommerce PRODUCT data\.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| hits\.product\.productQuantity       | INTEGER    | The quantity of the product purchased\.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| hits\.product\.productRevenue        | INTEGER    | The revenue of the product, expressed as the value passed to Analytics multiplied by 10^6 \(e\.g\., 2\.40 would be given as 2400000\)\.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| hits\.product\.productSKU            | STRING     | Product SKU\.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| hits\.product\.v2ProductName         | STRING     | Product Name\.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

More detail about schema: https://support.google.com/analytics/answer/3437719?hl=en
## 4. Data Processing & Exploratory Data Analysis
~~~~sql
SELECT
      COUNT(fullVisitorId) row_num,
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
~~~~
| row\_num |
|----------|
| 467260   |

**UNNEST hits and products**
~~~~sql
SELECT
      date, 
      fullVisitorId,
      eCommerceAction.action_type,
      product.v2ProductName,
      product.productRevenue,
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`,
UNNEST(hits) AS hits,
UNNEST(hits.product) as product
~~~~

| date     | fullVisitorId       | action_type | v2ProductName                         | productRevenue |
|----------|---------------------|-------------|---------------------------------------|----------------|
| 20170712 | 4080810487624198636 | 1           | YouTube Custom Decals                 |                |
| 20170712 | 4080810487624198636 | 2           | YouTube Custom Decals                 |                |
| 20170712 | 7291695423333449793 | 1           | Keyboard DOT Sticker                  |                |
| 20170712 | 7291695423333449793 | 2           | Keyboard DOT Sticker                  |                |
| 20170712 | 3153380067864919818 | 2           | Google Baby Essentials Set            |                |
| 20170712 | 3153380067864919818 | 1           | Google Baby Essentials Set            |                |
| 20170712 | 5615263059272956391 | 0           | Android Lunch Kit                     |                |
| 20170712 | 5615263059272956391 | 0           | Android Rise 14 oz Mug                |                |
| 20170712 | 5615263059272956391 | 0           | Android Sticker Sheet Ultra Removable |                |
| 20170712 | 5615263059272956391 | 0           | Windup Android                        |                |
| ... | ... |...|...|...|
## 5. Ask questions and solve it
**5.1 Calculate total visits, pageview, transaction, and revenue for Jan, Feb, and March 2017**
~~~~sql
SELECT
      FORMAT_TIMESTAMP('%Y%m', TIMESTAMP(PARSE_DATE('%Y%m%d', DATE))) AS month,
      COUNT(totals.visits) AS visit,
      SUM(totals.pageviews) AS pageviews,
      SUM(totals.transactions) AS transactions
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*` 
WHERE _table_suffix BETWEEN '0101' AND '0131'
GROUP BY month
UNION ALL
SELECT
      FORMAT_TIMESTAMP('%Y%m', TIMESTAMP(PARSE_DATE('%Y%m%d', DATE))) AS month,
      COUNT(totals.visits) AS visit,
      SUM(totals.pageviews) AS pageviews,
      SUM(totals.transactions) AS transactions
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*` 
WHERE _table_suffix BETWEEN '0201' AND '0229'
GROUP BY month
UNION ALL
SELECT
      FORMAT_TIMESTAMP('%Y%m', TIMESTAMP(PARSE_DATE('%Y%m%d', DATE))) AS month,
      COUNT(totals.visits) AS visit,
      SUM(totals.pageviews) AS pageviews,
      SUM(totals.transactions) AS transactions
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*` 
WHERE _table_suffix BETWEEN '0301' AND '0331'
GROUP BY month
~~~~
**RESULT**
| month  | visit | pageviews | transactions  |
|--------|-------|-----------|---------------|
| 201701 | 64694 | 257708    | 713           |
| 201702 | 62192 | 233373    | 733           |
| 201703 | 69931 | 259522    | 993           |

**5.2 Bounce rate per traffic source in July 2017**
~~~~sql
SELECT
    trafficSource.source AS source,
    SUM(totals.visits) AS total_visits,
    SUM(totals.Bounces) AS total_no_of_bounces,
    (SUM(totals.Bounces)/SUM(totals.visits))* 100.00 AS bounce_rate
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`
GROUP BY source
ORDER BY total_visits DESC;
~~~~
**RESULT**
| Source                      | Total visits | Total no of bounces | Bounce rate |
| :-------------------------- | :----------- | :------------------ | :---------- |
| google                      | 38400        | 19798               | 51.557      |
| (direct)                    | 19891        | 8606                | 43.266      |
| youtube.com                 | 6351         | 4238                | 66.73       |
| analytics.google.com        | 1972         | 1064                | 53.955      |
| Partners                    | 1788         | 936                 | 52.349      |
| m.facebook.com              | 669          | 430                 | 64.275      |
| google.com                  | 368          | 183                 | 49.728      |
| dfa                         | 302          | 124                 | 41.06       |
| sites.google.com            | 230          | 97                  | 42.174      |
| facebook.com                | 191          | 102                 | 53.403      |
| reddit.com                  | 189          | 54                  | 28.571      |
| qiita.com                   | 146          | 72                  | 49.315      |
| baidu                       | 140          | 84                  | 60          |
| quora.com                   | 140          | 70                  | 50          |
| bing                        | 111          | 54                  | 48.649      |
| mail.google.com             | 101          | 25                  | 24.752      |
| yahoo                       | 100          | 41                  | 41          |
| blog.golang.org             | 65           | 19                  | 29.231      |
| l.facebook.com              | 51           | 45                  | 88.235      |
| groups.google.com           | 50           | 22                  | 44          |
| t.co                        | 38           | 27                  | 71.053      |
| google.co.jp                | 36           | 25                  | 69.444      |
| m.youtube.com               | 34           | 22                  | 64.706      |
| dealspotr.com               | 26           | 12                  | 46.154      |
| productforums.google.com    | 25           | 21                  | 84          |
| support.google.com          | 24           | 16                  | 66.667      |
| ...                         | ...          | ...                 | ...         |


**5.3 Revenue by traffic source by week, by month in June 2017**

~~~~sql
WITH 
month_data AS(
  SELECT
    "Month" AS time_type,
    format_date("%Y%m", parse_date("%Y%m%d", date)) AS month,
    trafficSource.source AS source,
    SUM(p.productRevenue)/1000000 AS revenue
  FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201706*`,
    unnest(hits) hits,
    unnest(product) p
  WHERE p.productRevenue IS NOT NULL
  GROUP BY 1,2,3
  ORDER BY revenue DESC
),

week_data AS(
  SELECT
    "Week" AS time_type,
    format_date("%Y%W", parse_date("%Y%m%d", date)) AS week,
    trafficSource.source AS source,
    SUM(p.productRevenue)/1000000 AS revenue
  FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201706*`,
    unnest(hits) hits,
    unnest(product) p
  WHERE p.productRevenue IS NOT NULL
  GROUP BY 1,2,3
  ORDER BY revenue DESC
)

SELECT * FROM month_data
UNION ALL
SELECT * FROM week_data
ORDER BY source, time_type
~~~~
**RESULT**

| time_type | month  | source                                    | revenue       |
| --------- | ------ | ----------------------------------------- | ------------- |
| Month     | 201706 | (direct)                                  | 97,333619695 |
| Week      | 201724 | (direct)                                  | 30,908909927 |
| Week      | 201722 | (direct)                                  | 6,888899975  |
| Week      | 201723 | (direct)                                  | 17,325679919 |
| Week      | 201726 | (direct)                                  | 14,91480995  |
| Week      | 201725 | (direct)                                  | 27,295319924 |
| Month     | 201706 | bing                                      | 13.98         |
| Week      | 201724 | bing                                      | 13.98         |
| Month     | 201706 | chat.google.com                           | 74.03         |
| Week      | 201723 | chat.google.com                           | 74.03         |
| ...       | ...    | ...                                       | ...           |



**5.4 Average number of pageviews by purchaser type (purchasers vs non-purchasers) in June, July 2017**
~~~~sql
-- Count pageviews and number of purchaser 2017
WITH
count_pageview_purchaser AS(
SELECT 
      FORMAT_TIMESTAMP('%Y%m', TIMESTAMP(PARSE_DATE('%Y%m%d', DATE))) AS month
      ,SUM(totals.pageviews) total_pageviews
      ,COUNT(distinct fullVisitorId) total_user
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
UNNEST (hits) hits,
UNNEST (hits.product) product
WHERE (_table_suffix BETWEEN '0601' AND '0731') AND (totals.transactions >= 1 AND product.productRevenue IS NOT NULL)
GROUP BY month),
count_pageview_non_purchaser AS
-- Count pageviews and number of non-purchaser
(SELECT 
      FORMAT_TIMESTAMP('%Y%m', TIMESTAMP(PARSE_DATE('%Y%m%d', DATE))) AS month
      ,SUM(totals.pageviews) total_pageviews
      ,COUNT(DISTINCT fullVisitorId) total_user
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
UNNEST (hits) hits,
UNNEST (hits.product) product
WHERE (_table_suffix BETWEEN '0601' AND '0731') AND (totals.transactions IS NULL AND product.productRevenue IS NULL)
GROUP BY month)
SELECT
      p.month
      ,p.total_pageviews/p.total_user as avg_pageviews_purchase
      ,np.total_pageviews/np.total_user as avg_pageviews_non_purchase
FROM count_pageview_purchaser p
INNER JOIN count_pageview_non_purchaser np
USING(MONTH)
ORDER BY p.month
~~~~

**RESULT**

| month  | avg_pageviews_purchase | avg_pageviews_non_purchase |
| ------ | ---------------------- | -------------------------- |
| 201706 | 94.02050113895217      | 316.86558846341671         |
| 201707 | 124.23755186721992     | 334.05655979568053         |

**5.5 Average amount of money spent per session. Only include purchaser data in July 2017**
~~~~sql
WITH count_transaction_per_user AS(
  SELECT
       FORMAT_TIMESTAMP('%Y%m', TIMESTAMP(PARSE_DATE('%Y%m%d', DATE))) AS Month
      ,COUNT(DISTINCT fullVisitorId) AS count_user
      ,SUM(totals.transactions) AS count_transactions
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
UNNEST (hits) hits,
UNNEST (hits.product) product
WHERE (_table_suffix BETWEEN '0701' AND '0731') AND (totals.transactions >=1 AND product.productRevenue IS NOT NULL)
GROUP BY Month)
SELECT
      MONTH 
      ,ROUND(count_transactions/count_user,9) AS Avg_total_transactions_per_user
FROM count_transaction_per_user
~~~~
**RESULT**

| MONTH  | Avg_total_transactions_per_user |
| ------ | ------------------------------- |
| 201707 | 4.163900415                     |

**5.6 Average amount of money spent per session. Only include purchaser data in July 2017**

~~~~sql
WITH cal_total_revenue AS(
  SELECT 
        FORMAT_TIMESTAMP('%Y%m', TIMESTAMP(PARSE_DATE('%Y%m%d', DATE))) AS month
        ,sum(product.productRevenue) AS total_revenue
        ,sum(totals.visits) AS total_visit
  FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
  UNNEST (hits) hits,
  UNNEST (hits.product) product
  WHERE (_table_suffix BETWEEN '0701' AND '0731') AND (totals.transactions IS NOT NULL AND product.productRevenue IS NOT NULL)
  group by month
)
SELECT month,
        ROUND((total_revenue/total_visit)/1000000,2) AS avg_spend_per_session 
FROM cal_total_revenue
~~~~
**RESULT**

| Month  | avg_revenue_by_user_per_visit |
| ------ | ----------------------------- |
| 201707 | 43.85                         |

**5.7 Other products purchased by customers who purchased product "YouTube Men's Vintage Henley" in July 2017. Output should show product name and the quantity was ordered.**

~~~~sql
SELECT 
      product.v2ProductName
      ,SUM(product.productQuantity) AS quantity
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
UNNEST(hits) hits,
UNNEST(hits.product) product
WHERE (_table_suffix BETWEEN '0701' AND '0731') AND (product.v2ProductName != "YouTube Men's Vintage Henley") AND (product.productRevenue IS NOT NULL) AND (fullVisitorId IN(
      SELECT 
            fullVisitorId 
      FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
      UNNEST(hits) hits,
      UNNEST(hits.product) product
      WHERE (_table_suffix BETWEEN '0701' AND '0731') AND (product.v2ProductName = "YouTube Men's Vintage Henley") AND (product.productRevenue IS NOT NULL)))
GROUP BY product.v2ProductName
ORDER BY quantity DESC
~~~~

**RESULT**

| v2ProductName                                    | quantity |
| ------------------------------------------------ | -------- |
| Google Sunglasses                                | 20       |
| Google Women's Vintage Hero Tee Black            | 7        |
| SPF-15 Slim & Slender Lip Balm                   | 6        |
| Google Women's Short Sleeve Hero Tee Red Heather | 4        |
| Google Men's Short Sleeve Badge Tee Charcoal     | 3        |
| YouTube Men's Fleece Hoodie Black                | 3        |
| Android Wool Heather Cap Heather/Black           | 2        |
| Crunch Noise Dog Toy                             | 2        |
| Red Shine 15 oz Mug                              | 2        |
| ...                                              | ...      |

**5.8 Calculate cohort map from product view to addtocart to purchase in Jan, Feb and March 2017. For example, 100% product view then 40% add_to_cart and 10% purchase.
Add_to_cart_rate = number product  add to cart/number product view. Purchase_rate = number product purchase/number product view. The output should be calculated in product level.**

~~~~sql
WITH
      number_productviews_tb as(
SELECT 
       FORMAT_TIMESTAMP('%Y%m', TIMESTAMP(PARSE_DATE('%Y%m%d', DATE))) AS month
      ,sum(case when eCommerceAction.action_type = '2' then 1 else 0 end) AS num_product_view
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
UNNEST (hits) hits
WHERE (_table_suffix BETWEEN '0101' AND '0331')
group by month),
number_addtocart_tb AS(SELECT 
       FORMAT_TIMESTAMP('%Y%m', TIMESTAMP(PARSE_DATE('%Y%m%d', DATE))) AS month
      ,sum(case when eCommerceAction.action_type = '3' then 1 else 0 end) AS num_addtocart
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
UNNEST (hits) hits
WHERE (_table_suffix BETWEEN '0101' AND '0331')
GROUP BY month),
num_purchase_tb AS(
SELECT 
       FORMAT_TIMESTAMP('%Y%m', TIMESTAMP(PARSE_DATE('%Y%m%d', DATE))) AS month
      ,sum(case when eCommerceAction.action_type = '6' then 1 else 0 end) as num_purchase
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
UNNEST (hits) hits,
UNNEST (hits.product) product
WHERE (_table_suffix BETWEEN '0101' AND '0331') AND (product.productRevenue IS NOT NULL)
GROUP BY month)
SELECT
       pv.month
      ,pv.num_product_view
      ,atc.num_addtocart
      ,pc.num_purchase
      ,round(100.00*(atc.num_addtocart/pv.num_product_view),2) AS add_to_cart_rate
      ,round(100.00*(pc.num_purchase/pv.num_product_view),2) AS purchase_rate
FROM number_productviews_tb pv
JOIN number_addtocart_tb atc ON pv.month=atc.month
JOIN num_purchase_tb pc ON pc.month=pv.month
ORDER BY month
~~~~

**RESULT**

| month  | num_product_view | num_addtocart | num_purchase | add_to_cart_rate | purchase_rate |
| ------ | ---------------- | ------------- | ------------ | ---------------- | ------------- |
| 201701 | 25787            | 7342          | 2143         | 28.47            | 8.31          |
| 201702 | 21489            | 7360          | 2060         | 34.25            | 9.59          |
| 201703 | 23549            | 8782          | 2977         | 37.29            | 12.64         |

##6. Conclusion
