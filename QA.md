What are your risk areas? Identify and describe them.

- Duplicate data was a big risk with this dataset. 
- Large disconnected tables were problematic.
- Inaccruate revenue figures caused concerns for sales data.

QA Process:
- To eliminate the concern of duplicate data, all raw files were converted to raw tables, then base tables using `SELECT DISTINCT`. 
- The larger tables were broken apart into smaller tables using DISTINCT row to avoid duplication 
- for all tables, counts were institued to ensure as many duplicates were removed as possible. 

##Example QA SQL code
```sql
SELECT 
	visit_id
	,full_visitor_id
	,units_sold
	,unit_price
	,COUNT(*) AS count_row
FROM base_revenue
GROUP BY 
	visit_id
	,full_visitor_id
	,units_sold
	,unit_price
HAVING COUNT(*) > 1

```
