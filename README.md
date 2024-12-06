# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
The goal of this project was to use raw data from provided CSV files and turn this into a functional database to answer analytical questions.

## Process
### Process CSVs
The first step was to turn the CSVs into a database featuring raw tables in a PostgreSQL server. This was done by improting the CSVs with no regard for data types; all types were provided as STRING types to ensure there was no conversion or loss of data
### Convert to BASE tables
Converting to base tables was done to convert all data into correct data types while also cleaning the data by removing duplicates. From big tables, such as `all_sessions` and `analytics` smaller tables were created to isolate the data and avoid duplication. An example of this was to extract a `base_visitors` table to capture all unique visitors in our dataset.

## Results
This dataset was fairly large, but very unorganized. Much of it was confusing, requiring some assumptions about what columns meant. While website visits were well documented, sales were not. This resulted in good analytics regarding site visits but I would not trust any data that was relating to actual sales. 

## Challenges 
Sales data was difficult to attain from this data set. Individual product information could not be attributed to transactions nor could unit price be reconciled to products. 

## Future Goals
With more time, and perhaps more guidance on what the information meant, the accuracy of the sales data could be far greater. Given enough time, reconciling traffic data with sales_by_sku could be done to confirm revenue data.
