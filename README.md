# retail-360-capstone
AWS Retail Customer 360 and Churn Prediction — Capstone Project (Course Kit)
One-line summary
A customer intelligence project that unifies retail data (transactions, marketing, and support) into a governed data lake on AWS. Students build a Customer 360 dataset and visualize churn trends while preparing data for downstream ML models.
Learning objectives
·      Load and unify customer, sales, support, and marketing data from multiple sources.
·      Use AWS Glue ETL to clean and enrich data for Customer 360 analytics.
·      Store curated data as Iceberg tables governed by Lake Formation.
·      Query the unified dataset using Athena and Redshift Spectrum.
·      Visualize churn indicators with QuickSight dashboards.
Architecture (high level)
 Source CSVs (Sales, Support, Marketing, Customer) --> S3 Raw Zone
                                                    |
                                              Glue Crawlers & Catalog
                                                    |
                                      AWS Glue ETL (PySpark) --> S3 Curated (Iceberg tables)
                                                    |
                                      +-------------------+------------------+
                                      |                                     |
                                   Athena                         Redshift Spectrum
                                      |                                     |
                                      +---------> QuickSight Dashboards <----+
 Lake Formation governs access and masking (PII fields)
Services used (short)
·      AWS Glue Connectors / S3 Batch Upload — Load CSV data from source systems.
·      Amazon S3 — Data lake for raw and curated layers.
·      AWS Glue (PySpark) — Data cleaning, joins, and enrichment.
·      Apache Iceberg — Lakehouse storage with ACID updates.
·      Amazon Athena / Redshift Spectrum — Analytical querying.
·      Amazon QuickSight — Visualization of churn metrics.
·      AWS Lake Formation — Governance and column masking policies.
Sample datasets
 **customer_master.csv**
 customer_id,name,region,signup_date,email
 C001,Ananya Rao,South,2022-04-01,ananya@retaildemo.com
 C002,John Smith,West,2021-09-12,john@retaildemo.com
 C003,Meena Iyer,North,2023-02-20,meena@retaildemo.com
 **sales.csv**
 order_id,customer_id,order_date,amount,channel
 O101,C001,2025-10-01,250,Online
 O102,C002,2025-09-25,180,Store
 **support.csv**
 ticket_id,customer_id,category,status,created_date
 T01,C001,Payment,Closed,2025-09-28
 T02,C003,Delivery,Open,2025-10-03
Data Quality Rules:
Use PySpark StructType for all source datasets:
Customer Master Rules
CustomerID must be non-null, numeric
Email should match regex format
Phone should be 10 digits
SignupDate must parse to TimestampType
Orders Dataset Rules
OrderID must be unique
OrderDate must be a valid timestamp
Amount must be > 0
ProductID must exist in product master (referential integrity)
Website Clickstream Rules
URL must be non-null
EventType must be one of: PAGE_VIEW, ADD_TO_CART, PURCHASE
SessionID cannot be null
Feedback/Reviews Rules
Rating in range 1–5
Feedback text cannot be empty
Data Integrity & Consistency Checks
Add mandatory checks:
Duplicate detection
Null percentage threshold (<5%)
Invalid email/phone filtering
Order–Customer foreign key match
Time travel checks
(OrderDate ≥ SignupDate)
Data Quality Score Generation
valid_records / total_records * 100 → data_quality_score
Dashboard Metrics
Customer360 KPI Categories
1. Volume KPIs
Total customers
Total orders
Active vs inactive customers (last 30/60/90 days)
2. Performance KPIs
Average order value (AOV)
Customer Lifetime Value (CLV)
Revenue per customer segment
3. Customer Health KPIs
Churn probability category (High, Medium, Low)
Number of customers at churn risk
Retained customers after last campaign

4. Engagement KPIs (Only digital funnel basics)
Conversion rate (view → purchase)
Add-to-cart rate
 
5. Trend Analytics
Revenue by month
Order count trend
Visualization Types
Heatmap: Customer engagement vs churn probability
Funnel: Website → cart → purchase
Churn TreeMap: Segments with highest churn
Line Chart: Revenue & CLV trends
Bar Chart: Top 10 products by repeat purchase
Monitoring & Observability
CloudWatch logs for Glue ETL
Firehose delivery error alerts
Glue job failure alarms

Governance Using Lake Formation
Fine-grained table permissions
Marketing team → can view customer segments
Customer support → limited PII access
Analytics team → full access to curated zone
Column-level masking
 Sensitive columns:
Email
Phone
Address
Masked using:
email_masked,
phone_last4, etc.
Tag-based access control
 Tag datasets with:
PII = TRUE
SegmentData = TRUE
Critical = TRUE
Step-by-step lab setup (abridged)
·      Create S3 buckets for raw and curated data.
·      Upload source CSVs (sales, marketing, support, customers).
·      Run Glue Crawlers to register raw data schemas.
·      Develop Glue ETL job to clean, join, and enrich datasets.
·      Write Customer 360 dataset as Iceberg tables governed by Lake Formation.
·      Query data in Athena and build churn-risk dashboards in QuickSight.
Assessment tasks (exercises for students)
·      Compute average order value and ticket count per customer.
·      Identify top 10 customers by lifetime value (LTV).
·      Create QuickSight visualization showing churn probability by region.
·      Add a new column 'days_since_last_purchase' using Glue PySpark.
Security & best practices
·      Mask email and personal identifiers using Lake Formation policies.
·      Partition Iceberg tables by region and signup_year.
·      Encrypt all S3 data with KMS.
·      Enable Glue bookmarks to avoid duplicate records.
·      Use Athena workgroups to track query cost per student.
Expected Outcomes
Unified Customer 360 dataset enabling churn analytics and business insights, serving as a foundation for future ML models.
Amazon SageMaker Predictive Modeling
Extend this project by using Amazon SageMaker to build a churn-prediction model based on the Customer 360 dataset. Use features like avg_order_value, ticket_count, and region to train the model, and visualize predicted churn in QuickSight.
