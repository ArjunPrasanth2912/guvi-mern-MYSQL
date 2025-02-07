GUVI-MERN-MYsql task

Task -details
SQL Task 

	
Task:
Create a database and tables to manage a simple e-commerce system. 
The system should have three tables: customers, orders, and products.

Requirements:

Create a database named ecommerce.
Create three tables: customers, orders, and products.
Insert some sample data into the tables.

Table Structure:

customers
id (primary key, auto-increment): unique identifier for each customer
name: customer's name
email: customer's email address
address: customer's address

orders
id (primary key, auto-increment): unique identifier for each order
customer_id (foreign key referencing customers.id): a customer who placed the                               order
order_date: date the order was placed
total_amount: total amount of the order

products
id (primary key, auto-increment): unique identifier for each product
name: product's name
price: product's price
description: product's description


Queries to Write:

Retrieve all customers who have placed an order in the last 30 days.
Get the total amount of all orders placed by each customer.
Update the price of Product C to 45.00.
Add a new column discount to the products table.
Retrieve the top 3 products with the highest price.
Get the names of customers who have ordered Product A.
Join the orders and customers tables to retrieve the customer's name and order date for each order. 
Retrieve the orders with a total amount greater than 150.00.
Normalize the database by creating a separate table for order items and updating the orders table to reference the order_items table.
Retrieve the average total of all orders.


Note:
Use MySQL syntax and formatting for your queries.
Provide clear and concise comments for each query explaining what it does.



