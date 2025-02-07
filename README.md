MYSQL task
----
-- creating the database
create schema ecommerce;
use ecommerce;

-- creating tables

create table customers(
id int auto_increment primary key,
name varchar(255) not null,
email varchar(255) not null,
address text not null
);

create table orders(
id int auto_increment primary key,
customer_id int,
order_date date not null,
total_amount decimal(10,2) not null, -- 10 digits 2 after the decimal point
FOREIGN KEY (customer_id) REFERENCES customers(id)
);

create table products(
id int auto_increment primary key,
name varchar(255) not null,
price decimal(10,2) not null,
description text
);


-- inserting data into the tables

insert into customers (name,email,address) values
("arjun","arjun@gmail.com","chennai"),
("raja","raja@gmail.com","kumbakonam"),
("yogesh","yogesh@gmail.com","Madurai"),
("srini","srini@gmail.com","Nellai"),
("bharat","bharat@gmail.com","Kovai");

  -- select * from customers;
 
 insert into orders(customer_id,order_date,total_amount)values
 (1,curdate() - interval 10 day,200),
 (2,curdate() - interval 20 day,280),
 (3,curdate() - interval 30 day,300),
 (4,curdate() - interval 35 day,50),
 (5,curdate() - interval 30 day,150);
 
 -- select * from orders;
 
 insert into products (name,price,description) values
 ("prdA",150,"premium"),
 ("prdB",200,"cheap"),
 ("prdC",50,"Newest"),
 ("prdD",30,"Latest"),
 ("prdE",250,"BestSelling");
 
 -- select * from products;
 
 


-- queries

-- 1)Retrieve all customers who have placed an order in the last 30 days

SELECT distinct c.id, c.name, c.email,c.address
FROM customers c
JOIN orders o ON c.id = o.customer_id
WHERE o.order_date >= CURDATE() - INTERVAL 30 DAY;



-- 2)Get the total amount of all orders placed by each customer
 
 select c.id ,c.name,sum(o.total_amount) from 
 customers c left join orders o on
 c.id=o.customer_id
 group by c.id,c.name;

 

-- 3)Update the price of Product C to 45.00.

SET SQL_SAFE_UPDATES = 0;
  update products
  set price = 45.00
  where name = "prdC";
  
    -- select * from products;

    
   
   -- 4) Add a new column discount to the products table.
   
   alter table products
   add column discount decimal(10,2) default 0.00;

   
   
   -- 5) Retrieve the top 3 products with the highest price.
   
  select * from products order by price desc limit 3;

  
  
  -- 6) Get the names of customers who have ordered Product A.
  
     ALTER TABLE orders
ADD COLUMN product_id INT,
ADD FOREIGN KEY (product_id) REFERENCES products(id);


UPDATE orders SET product_id = 1 WHERE id = 1;  -- Order 1 - Product A
UPDATE orders SET product_id = 2 WHERE id = 2;  -- Order 2 - Product B
UPDATE orders SET product_id = 3 WHERE id = 3;  -- Order 3 - Product C
UPDATE orders SET product_id = 4 WHERE id = 4;  -- Order 4 - Product D
UPDATE orders SET product_id = 5 WHERE id = 5;  -- Order 5 - Product E

SELECT DISTINCT c.name
FROM customers c
JOIN orders o ON c.id = o.customer_id
JOIN products p ON o.product_id = p.id
WHERE p.name = "prdA";


  
-- 7)Join the orders and customers tables to retrieve the customer's name and order date for each order. 

select c.name,o.order_date from customers c join
orders o on c.id=o.customer_id;



-- 8) Retrieve the orders with a total amount greater than 150.00.

select * from orders where total_amount>150.00;



-- 9)Normalize the database by creating a separate table for order items and updating the orders table to reference the order_items table.

	CREATE TABLE order_items (
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT NOT NULL,
    price DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (order_id) REFERENCES orders(id) ON DELETE CASCADE,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE
);

select * from order_items;

 -- Now we have the order_items table to store products per order, we should remove the total_amount column from the orders table. 
-- This is because the total amount can now be calculated dynamically from the order_items table.

ALTER TABLE orders
DROP COLUMN total_amount;



-- 10) Retrieve the average total of all orders.

	SELECT AVG(oi.quantity * oi.price) AS avg_order_value
    FROM order_items oi;

