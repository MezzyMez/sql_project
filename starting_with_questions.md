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



Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







