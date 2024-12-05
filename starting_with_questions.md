Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

```sql
SELECT 
	country
	,SUM(revenue) AS total_revenue
FROM base_revenue

JOIN base_visitors USING (full_visitor_id)

WHERE country IS NOT NULL

GROUP BY
	country

ORDER BY total_revenue DESC
```

```sql
SELECT 
	city
	,SUM(revenue) AS total_revenue
FROM base_revenue

JOIN base_visitors USING (full_visitor_id)

WHERE city IS NOT NULL
	AND city != 'n/a'

GROUP BY
	city

ORDER BY total_revenue DESC
```

Answer:
The countries with the most revenue are the United States, Canada and Germany.
The cities with the most revenue are Mountain View, San Bruno and New York.




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

```sql
SELECT country
	,AVG(units_sold)::NUMERIC(10,2) AS average_units_sold
FROM base_revenue
JOIN base_visitors USING (full_visitor_id)
WHERE country IS NOT NULL

GROUP BY country
ORDER BY average_units_sold DESC
```



Answer:
The average number of units sold is highest in the United States with 16.8, then Canada with 2.17 and Germany, Japan and Switzerland with 1.




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
```sql
SELECT 
	country
	,city
	,product_category
	,COUNT(*) AS count
FROM base_transactions
JOIN base_visitors USING (full_visitor_id)
WHERE country IS NOT NULL
GROUP BY country, city, product_category
ORDER BY count DESC
```


Answer:
In general, there are not enough confirmed sales to derive accurate answers. Products in the home category make the vast bulk of sales, with NEST products containing the most common sales.





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

```sql
SELECT 
	country
	,product_name
	,COUNT(product_name)
	,RANK() OVER (PARTITION BY country ORDER BY COUNT(product_name) DESC)
FROM base_transactions
JOIN base_visitors AS visitors USING (full_visitor_id)
GROUP BY country, product_name
ORDER BY country
```

Answer:

"Australia"	"Nest® Cam Indoor Security Camera - USA"	1	1
"Canada"	"Google Men's 3/4 Sleeve Raglan Henley Grey"	1	1
"Israel"	"Nest® Protect Smoke + CO Black Wired Alarm-USA"	1	1
"Switzerland"	"YouTube Men's 3/4 Sleeve Henley"	1	1
"United States"	"Nest® Learning Thermostat 3rd Gen-USA - Stainless Steel"	7	1



**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







