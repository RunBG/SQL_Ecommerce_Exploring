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
Dataset schama: https://support.google.com/analytics/answer/3437719?hl=en
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
## 7. Conclusion
