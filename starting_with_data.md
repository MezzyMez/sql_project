Question 1: What is the average time on site?

SQL Queries:

Answer: 



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
| GGOEGOAQ012899 | 456 | Ballpoint LED Light Pen |
| GGOEGDHC074099 | 334 | Google 17oz Stainless Steel Sport Bottle |
| GGOEGOCB017499 | 319 | Leatherette Journal |
| GGOEGOCC077999 | 290 | Google Spiral Journal with Pen |
| GGOEGFYQ016599 | 253 | Koozie Can Kooler |

Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
