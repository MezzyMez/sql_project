Question 1: What is the average time on site?

SQL Queries:

```sql
SELECT 
	AVG (time_on_site)::NUMERIC(10,2) AS average_time_on_site
FROM base_visits
```

Answer: 
 The average time on site for all sessions was 342 seconds


Question 2: What are the top 5 countries with highest average time on site?

SQL Queries:

```sql
SELECT 
	country
	,AVG(time_on_site)::NUMERIC(10,2) AS average_time_on_site
	,RANK() OVER (ORDER BY AVG(time_on_site) DESC)
FROM base_visits
JOIN base_visitors USING (full_visitor_id)
WHERE 1=1 
	AND country != '(not set)'
GROUP BY country
HAVING AVG(time_on_site) IS NOT NULL
```

Answer:

| Country | Average Time On Site | Rank |
| ------- | -------------------- | ---- |
| Sint Maarten | 766.00 | 1 |
| Belarus |	656.00 | 2 |
| Finland |	635.33 | 3 |
| Taiwan | 614.48 | 4 |
| Tunisia |	574.00 | 5 |


Question 3: What are the names of products of the top 5 product_skus ordered?

SQL Queries:

```sql
SELECT DISTINCT 
	sbs.*
	,p.product_name
FROM base_sales_by_sku AS sbs
LEFT JOIN base_products AS p USING (product_sku)
ORDER BY total_ordered DESC
LIMIT 5
```

Answer:

| Product SKU | Quantity | Product Name |
| ----------- | -------- | ------------ |
| GGOEGOAQ012899 | 456 | Ballpoint LED Light Pen |
| GGOEGDHC074099 | 334 | Google 17oz Stainless Steel Sport Bottle |
| GGOEGOCB017499 | 319 | Leatherette Journal |
| GGOEGOCC077999 | 290 | Google Spiral Journal with Pen |
| GGOEGFYQ016599 | 253 | Koozie Can Kooler |


Question 4: What are the 3 categories with the top sentiment scores weighted by quantity ordered?

SQL Queries:

```sql
SELECT 
	product_category
	,(SUM(sentiment_score*ordered_quantity)/SUM(ordered_quantity))::NUMERIC(10,2) AS average_sentiment_score
FROM base_products
GROUP BY product_category
ORDER BY average_sentiment_score DESC
LIMIT 3
```

Answer:

| Product Category | Sentiment Score |
| ---------------- | --------------- |
| Drinkware | 0.90 |
| Lifestyle | 0.73 |
| Home/Limited Supply |	0.72 |


Question 5: What countries have the highest visits (ignoring the United States)?

SQL Queries:

```sql
SELECT 
	country
	,COUNT(country) AS user_count
FROM base_visitors
WHERE 1=1
	AND country IS NOT NULL
	AND country != 'United States'
GROUP BY country
ORDER BY user_count DESC
LIMIT 3
```

Answer:

| Country | Visits |
| ------- | ------ |
| India | 694 |
| United Kingdom | 641 |
| Canada | 609 |