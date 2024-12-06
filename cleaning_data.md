What issues will you address by cleaning the data?

- Original data will be left untouched within "raw" tables. New tables will be created as "base" tables with cleaned versions
- Correcting data types by casting correct types
- sales_report contains all duplicate information. Remove this table and recreate as VIEW using other tables
- analytics table: remove duplicates
- create base_visitors table
- remove "not available in demo dataset" from city, replace with N/A
- remove user_id from analytics (no values present)
- remove social_engagement_type (only one value present)
- Standardize column names between tables
- Removing duplicate products 
- Remove products with 0 stock & 0 orders
- Correct unit_price (divide by 1,000,000)
- Change visit_start_time to timestamp
- Create base_visits table for unique visits



Queries:
Below, provide the SQL queries you used to clean your data.

##Create base_analytics
```sql
DROP TABLE IF EXISTS base_analytics;

CREATE TABLE base_analytics AS

SELECT DISTINCT 
	visit_number::INT
	,visit_id::INT
	,TO_TIMESTAMP(visit_start_time::INT) AS visit_start_time
	,date::DATE
	,full_visitor_id
	,channel_grouping
	,units_sold::INT
	,pageviews::INT
	,time_on_site::INT
	,bounces::INT
	,revenue::BIGINT
	,unit_price::FLOAT / 1000000 AS unit_price
FROM raw_analytics;
```

##Create base_products
```sql
DROP TABLE IF EXISTS base_products;

CREATE TABLE base_products AS 

SELECT DISTINCT
	p.sku AS product_sku
	,COALESCE(s.v2_product_name, p.name) AS product_name
	,s.v2_product_category AS product_category
	,p.ordered_quantity::INT
	,p.stock_level::INT
	,p.restocking_lead_time::INT
	,p.sentiment_score::FLOAT
	,p.sentiment_magnitude::FLOAT
FROM
	raw_products AS p

LEFT JOIN raw_all_sessions AS s 
	ON p.sku = s.product_sku

WHERE 
	ordered_quantity::INT > 0
	AND stock_level::INT > 0;
```

##Create base_revenue
```sql
DROP TABLE IF EXISTS base_revenue;

CREATE TABLE base_revenue AS 

SELECT DISTINCT
	visit_id
	,full_visitor_id
	,units_sold::INT
	,(unit_price::BIGINT)/1000000 AS unit_price
	,((revenue::FLOAT / (units_sold::FLOAT * unit_price::FLOAT))-1)::NUMERIC(10,3) AS tax
	,(revenue::BIGINT)/1000000 AS revenue
	--,units_sold::INT * unit_price::BIGINT AS revenue_alt 
FROM raw_analytics
WHERE 
	revenue IS NOT NULL
ORDER BY visit_id
```

##Create base_sales_by_sku
```sql
CREATE TABLE base_sales_by_sku AS

SELECT 
	product_sku
	,total_ordered::INT
FROM raw_sales_by_sku
```

##Create base_transactions
```sql
DROP TABLE IF EXISTS base_transactions;

CREATE TABLE base_transactions AS 

	SELECT 
		full_visitor_id
		,total_transaction_revenue::FLOAT/1000000 AS total_transactions_revenue
	FROM raw_all_sessions
	WHERE total_transaction_revenue IS NOT NULL;
```

##Create base_visitors
```sql
DROP TABLE IF EXISTS base_visitors;

CREATE TABLE base_visitors AS

SELECT DISTINCT
	full_visitor_id
	,country
	,CASE
		WHEN city = 'not available in demo dataset' THEN 'n/a'
		WHEN city = '(not set)' THEN NULL
	 	ELSE city
	 END AS city
	 ,NULL AS user_id
FROM raw_all_sessions

UNION 

SELECT DISTINCT
	full_visitor_id
	,NULL AS country
	,NULL as city
	,user_id
FROM raw_analytics

ORDER BY full_visitor_id
```

##Create base_visits
```sql
DROP TABLE IF EXISTS base_visits;

CREATE TABLE base_visits AS
	SELECT DISTINCT
	visit_number
	,visit_id
	,visit_start_time
	,date
	,full_visitor_id
	,channel_grouping
	,pageviews
	,time_on_site
	FROM base_analytics
	ORDER BY visit_id
```