# SQL_Ecommerce_Exploring

## 1. Introduction and Motivation
The eCommerce dataset is stored in a public Google BigQuery dataset. This dataset contains information about user sessions on a website collected from Google Analytics in 2017.

Based on the eCommerce dataset, the author perform queries to analyze website activity in 2017, such as calculating bounce rate, identifying days with the highest revenue, analyzing user behavior on pages, and various other types of analysis. This project aims to have an outlook on the business situation, marketing activity efficiency analyzing the products.

To query and work with this dataset, the author uses the Google BigQuery tool to write and execute SQL queries.
## 2. The goal of creating this project
- Overview of website activity
- Product analysis
- Revenue analysis
- Transactions analysis
- Type purchaser analysis
- Bounce rate analysis
## 3. Import raw data
The eCommerce dataset is stored in a public Google BigQuery dataset. To access the dataset, follow these steps:
- Log in to your Google Cloud Platform account and create a new project.
- Navigate to the BigQuery console and select your newly created project.
- Select "Add Data" in the navigation panel and then "Search a project".
- Enter the project ID **"bigquery-public-data.google_analytics_sample.ga_sessions"** and click "Enter".
- Click on the **"ga_sessions_"** table to open it.
## 4. Read and explain dataset
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
## 5. Data Processing & Exploratory Data Analysis
~~~~sql
SELECT COUNT(fullVisitorId) row_num,
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
~~~~
| row\_num |
|----------|
| 467260   |

**UNNEST hits and products**
~~~~sql
SELECT date, 
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
## 6. Ask questions and solve it
**6.1 Calculate total visits, pageview, transaction, and revenue for Jan, Feb, and March 2017**
~~~~sql
SELECT FORMAT_TIMESTAMP('%Y%m', TIMESTAMP(PARSE_DATE('%Y%m%d', DATE))) AS month,
COUNT(totals.visits) AS visit,
SUM(totals.pageviews) AS pageviews,
SUM(totals.transactions) AS transactions
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*` 
WHERE _table_suffix BETWEEN '0101' AND '0131'
GROUP BY month
UNION ALL
SELECT FORMAT_TIMESTAMP('%Y%m', TIMESTAMP(PARSE_DATE('%Y%m%d', DATE))) AS month,
COUNT(totals.visits) AS visit,
SUM(totals.pageviews) AS pageviews,
SUM(totals.transactions) AS transactions
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*` 
WHERE _table_suffix BETWEEN '0201' AND '0229'
GROUP BY month
UNION ALL
SELECT FORMAT_TIMESTAMP('%Y%m', TIMESTAMP(PARSE_DATE('%Y%m%d', DATE))) AS month,
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
**6.2 Bounce rate per traffic source in July 2017**
~~~~sql
SELECT trafficSource.source AS source
      ,sum(totals.visits) AS total_visits
      ,sum(totals.bounces) AS total_no_of_bounces
      ,ROUND(100.00*sum(totals.bounces)/sum(totals.visits),3) AS bounce_rate
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
WHERE _table_suffix BETWEEN '0701' AND '0731'
GROUP BY source
ORDER BY total_visits desc
~~~~
**RESULT**
| source                                                            | total_visits | total_no_of_bounces | bounce_rate |
| ----------------------------------------------------------------- | ------------ | ------------------- | ----------- |
| google                                                            | 38400        | 19798               | 51.557      |
| (direct)                                                          | 19891        | 8606                | 43.266      |
| [youtube.com](http://youtube.com)                                 | 6351         | 4238                | 66.73       |
| [analytics.google.com](http://analytics.google.com)               | 1972         | 1064                | 53.955      |
| Partners                                                          | 1788         | 936                 | 52.349      |
| [m.facebook.com](http://m.facebook.com)                           | 669          | 430                 | 64.275      |
| [google.com](http://google.com)                                   | 368          | 183                 | 49.728      |
| dfa                                                               | 302          | 124                 | 41.06       |
| [sites.google.com](http://sites.google.com)                       | 230          | 97                  | 42.174      |
| [facebook.com](http://facebook.com)                               | 191          | 102                 | 53.403      |
| [reddit.com](http://reddit.com)                                   | 189          | 54                  | 28.571      |
| [qiita.com](http://qiita.com)                                     | 146          | 72                  | 49.315      |
| baidu                                                             | 140          | 84                  | 60          |
| [quora.com](http://quora.com)                                     | 140          | 70                  | 50          |
| bing                                                              | 111          | 54                  | 48.649      |
| [mail.google.com](http://mail.google.com)                         | 101          | 25                  | 24.752      |
| yahoo                                                             | 100          | 41                  | 41          |
| [blog.golang.org](http://blog.golang.org)                         | 65           | 19                  | 29.231      |
| [l.facebook.com](http://l.facebook.com)                           | 51           | 45                  | 88.235      |
| [groups.google.com](http://groups.google.com)                     | 50           | 22                  | 44          |
| [t.co](http://t.co)                                               | 38           | 27                  | 71.053      |
| [google.co.jp](http://google.co.jp)                               | 36           | 25                  | 69.444      |
| [m.youtube.com](http://m.youtube.com)                             | 34           | 22                  | 64.706      |
| [dealspotr.com](http://dealspotr.com)                             | 26           | 12                  | 46.154      |
| [productforums.google.com](http://productforums.google.com)       | 25           | 21                  | 84          |
| [support.google.com](http://support.google.com)                   | 24           | 16                  | 66.667      |
| ask                                                               | 24           | 16                  | 66.667      |
| [int.search.tb.ask.com](http://int.search.tb.ask.com)             | 23           | 17                  | 73.913      |
| [optimize.google.com](http://optimize.google.com)                 | 21           | 10                  | 47.619      |
| [docs.google.com](http://docs.google.com)                         | 20           | 8                   | 40          |
| [lm.facebook.com](http://lm.facebook.com)                         | 18           | 9                   | 50          |
| [l.messenger.com](http://l.messenger.com)                         | 17           | 6                   | 35.294      |
| [adwords.google.com](http://adwords.google.com)                   | 16           | 7                   | 43.75       |
| [duckduckgo.com](http://duckduckgo.com)                           | 16           | 14                  | 87.5        |
| [google.co.uk](http://google.co.uk)                               | 15           | 7                   | 46.667      |
| [sashihara.jp](http://sashihara.jp)                               | 14           | 8                   | 57.143      |
| [lunametrics.com](http://lunametrics.com)                         | 13           | 8                   | 61.538      |
| [search.mysearch.com](http://search.mysearch.com)                 | 12           | 11                  | 91.667      |
| [outlook.live.com](http://outlook.live.com)                       | 10           | 7                   | 70          |
| [tw.search.yahoo.com](http://tw.search.yahoo.com)                 | 10           | 8                   | 80          |
| [phandroid.com](http://phandroid.com)                             | 9            | 7                   | 77.778      |
| [connect.googleforwork.com](http://connect.googleforwork.com)     | 8            | 5                   | 62.5        |
| [plus.google.com](http://plus.google.com)                         | 8            | 2                   | 25          |
| [m.yz.sm.cn](http://m.yz.sm.cn)                                   | 7            | 5                   | 71.429      |
| [google.co.in](http://google.co.in)                               | 6            | 3                   | 50          |
| [search.xfinity.com](http://search.xfinity.com)                   | 6            | 6                   | 100         |
| [google.ru](http://google.ru)                                     | 5            | 1                   | 20          |
| [s0.2mdn.net](http://s0.2mdn.net)                                 | 5            | 3                   | 60          |
| [hangouts.google.com](http://hangouts.google.com)                 | 5            | 1                   | 20          |
| [online-metrics.com](http://online-metrics.com)                   | 5            | 2                   | 40          |
| [m.sogou.com](http://m.sogou.com)                                 | 4            | 3                   | 75          |
| [away.vk.com](http://away.vk.com)                                 | 4            | 3                   | 75          |
| [in.search.yahoo.com](http://in.search.yahoo.com)                 | 4            | 2                   | 50          |
| [googleads.g.doubleclick.net](http://googleads.g.doubleclick.net) | 4            | 1                   | 25          |
| [getpocket.com](http://getpocket.com)                             | 3            |                     |             |
| [siliconvalley.about.com](http://siliconvalley.about.com)         | 3            | 2                   | 66.667      |
| [m.baidu.com](http://m.baidu.com)                                 | 3            | 2                   | 66.667      |
| [google.it](http://google.it)                                     | 2            | 1                   | 50          |
| [google.co.th](http://google.co.th)                               | 2            | 1                   | 50          |
| [wap.sogou.com](http://wap.sogou.com)                             | 2            | 2                   | 100         |
| [msn.com](http://msn.com)                                         | 2            | 1                   | 50          |
| [calendar.google.com](http://calendar.google.com)                 | 2            | 1                   | 50          |
| [amp.reddit.com](http://amp.reddit.com)                           | 2            | 1                   | 50          |
| [uk.search.yahoo.com](http://uk.search.yahoo.com)                 | 2            | 1                   | 50          |
| [search.1and1.com](http://search.1and1.com)                       | 2            | 2                   | 100         |
| [google.cl](http://google.cl)                                     | 2            | 1                   | 50          |
| [moodle.aurora.edu](http://moodle.aurora.edu)                     | 2            | 2                   | 100         |
| [au.search.yahoo.com](http://au.search.yahoo.com)                 | 2            | 2                   | 100         |
| [m.sp.sm.cn](http://m.sp.sm.cn)                                   | 2            | 2                   | 100         |
| [plus.url.google.com](http://plus.url.google.com)                 | 2            |                     |             |
| [myactivity.google.com](http://myactivity.google.com)             | 2            | 1                   | 50          |
| [github.com](http://github.com)                                   | 2            | 2                   | 100         |
| [centrum.cz](http://centrum.cz)                                   | 2            | 2                   | 100         |
| [google.nl](http://google.nl)                                     | 1            |                     |             |
| [google.es](http://google.es)                                     | 1            | 1                   | 100         |
| [kidrex.org](http://kidrex.org)                                   | 1            | 1                   | 100         |
| [newclasses.nyu.edu](http://newclasses.nyu.edu)                   | 1            |                     |             |
| [google.ca](http://google.ca)                                     | 1            |                     |             |
| [kik.com](http://kik.com)                                         | 1            | 1                   | 100         |
| [gophergala.com](http://gophergala.com)                           | 1            | 1                   | 100         |
| aol                                                               | 1            |                     |             |
| [earth.google.com](http://earth.google.com)                       | 1            |                     |             |
| [malaysia.search.yahoo.com](http://malaysia.search.yahoo.com)     | 1            | 1                   | 100         |
| [google.com.br](http://google.com.br)                             | 1            |                     |             |
| [suche.t-online.de](http://suche.t-online.de)                     | 1            | 1                   | 100         |
| [google.bg](http://google.bg)                                     | 1            | 1                   | 100         |
| [news.ycombinator.com](http://news.ycombinator.com)               | 1            | 1                   | 100         |
| [online.fullsail.edu](http://online.fullsail.edu)                 | 1            | 1                   | 100         |
| [it.pinterest.com](http://it.pinterest.com)                       | 1            | 1                   | 100         |
| [arstechnica.com](http://arstechnica.com)                         | 1            |                     |             |
| [es.search.yahoo.com](http://es.search.yahoo.com)                 | 1            | 1                   | 100         |
| [web.facebook.com](http://web.facebook.com)                       | 1            | 1                   | 100         |
| [ph.search.yahoo.com](http://ph.search.yahoo.com)                 | 1            |                     |             |
| [web.mail.comcast.net](http://web.mail.comcast.net)               | 1            | 1                   | 100         |
| [search.tb.ask.com](http://search.tb.ask.com)                     | 1            |                     |             |
| [images.google.com.au](http://images.google.com.au)               | 1            | 1                   | 100         |
| [mx.search.yahoo.com](http://mx.search.yahoo.com)                 | 1            | 1                   | 100         |

## 7. Conclusion
