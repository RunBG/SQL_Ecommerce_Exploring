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
| Field Name           | Data Type | Description                                                                                                                                                           |
|----------------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| fullVisitorId        | STRING    | The unique visitor ID.                                                                                                                                                |
| date                 | STRING    | The date of the session in YYYYMMDD format.                                                                                                                           |
| totals               | RECORD    | This section contains aggregate values across the session.                                                                                                            |
| totals.bounces       | INTEGER   | Total bounces (for convenience). For a bounced session, the value is 1, otherwise it is null.                                                                         |
| totals.hits          | INTEGER   | Total number of hits within the session.                                                                                                                              |
| totals.pageviews     | INTEGER   | Total number of pageviews within the session.                                                                                                                         |
| totals.visits        | INTEGER   | The number of sessions (for convenience). This value is 1 for sessions with interaction events. The value is null if there are no interaction events in the session.  |
| totals.transactions  | INTEGER   | Total number of ecommerce transactions within the session.                                                                                                            |
| trafficSource.source | STRING    | The source of the traffic source. Could be the name of the search engine, the referring hostname, or a value of the utm_source URL parameter.                         |


## 5. Data Processing & Exploratory Data Analysis
## 6. Ask questions and solve it
## 7. Conclusion
