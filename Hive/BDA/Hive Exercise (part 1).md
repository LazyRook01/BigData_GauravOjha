# Exercise 1
```
product_id,product_name,category,quantity,price_per_unit
1,Product A,Electronics,10,50.00
2,Product B,Clothing,15,25.00
3,Product C,Electronics,5,100.00
4,Product D,Clothing,8,30.00
```

1. Create a new database name it appropriately and use it.
2. Create a Hive table
3. Load data into the Hive table
4. Display the table
5. Calculate total revenue for each category and insert into the new table
6. Where in HDFS can one find the above table?
7.  Export the result to a local directory
***

# Exercise 2
orders.csv
```
order_id,customer_id,order_date,total_amount
1,101,2023-01-15,150.00
2,102,2023-01-20,200.00
3,103,2023-02-05,75.00
4,104,2023-02-10,300.00
5,105,2023-03-01,120.00
```

customers.csv
```
customer_id,customer_name,city
101,Alice,New York
102,Bob,Los Angeles
104,David,Chicago
```

1. Create a new database name it appropriately and use it.
2. Create Hive tables for the above
3. Load data into the tables
4. Write HQL query to join tables and handle null values
5.  Export the result to a local directory

In this exercise, you create two tables: orders and customers. You load data from the provided CSV files into these tables. The exercise focuses on performing a left outer join between the orders and customers tables to retrieve the order information along with the customer names. The LEFT OUTER JOIN ensures that all orders are included in the result, even if there is no matching customer information.

The result includes columns for order_id, order_date, total_amount, customer_name, and city. If there is no matching customer information for an order, the customer_name and city columns will contain null values.

Finally, the result of the join query is exported to a local directory in CSV format using the INSERT OVERWRITE LOCAL DIRECTORY statement.

Remember to adjust the file paths and table/column names as needed based on your environment and data files.