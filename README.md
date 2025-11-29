# Flipkart-Sales-Analysis-SQL-Project

 Project Overview

In this project, i analyze customer orders, products, categories, payments, and shipping performance—similar to Flipkart or Amazon datasets.
i explore how customers shop, which products perform best, payment preferences, revenue trends, and delivery efficiency.

Database Structure

customers   -   Customer details
products    -	Product list with category & price
sales       -   Purchase transactions
payments    -	Payment type & status
shippings   -   Order delivery & shipping time

1.Database & Table Creation

--sql flipkart sales analysis
create database flipkart;

-- Customers table
CREATE TABLE customers (
    customer_id INTEGER PRIMARY KEY,
    customer_name VARCHAR(35),
    state VARCHAR(25)
);

-- Products table
CREATE TABLE products (
    product_id INTEGER PRIMARY KEY,
    product_name VARCHAR(45),
    price DOUBLE PRECISION,
    cogs DOUBLE PRECISION,
    category VARCHAR(25),
    brand VARCHAR(25)
);

-- Sales table
CREATE TABLE sales (
    order_id INTEGER PRIMARY KEY,
    order_date DATE,
    customer_id INTEGER,
    order_status VARCHAR(25),
    product_id INTEGER,
    quantity INTEGER,
    price_per_unit DOUBLE PRECISION
   
);

-- Shippings table
CREATE TABLE shippings (
    shipping_id INTEGER PRIMARY KEY,
    order_id INTEGER,
    shipping_date DATE,
    return_date DATE,
    shipping_providers VARCHAR(55),
    delivery_status VARCHAR(55)
    
);

-- Payments table
CREATE TABLE payments (
    payment_id INTEGER PRIMARY KEY,
    order_id INTEGER,
    payment_date DATE,
    payment_status VARCHAR(55)
    
);

select*from customers;
select*from payments;
select*from products;
select*from sales;
select*from shippings;

2. Data Exploration & Cleaning
--how many sales we have?
SELECT COUNT(*) FROM sales;

--how many unique customers we have?
SELECT COUNT(DISTINCT customer_id) FROM customers;
SELECT DISTINCT category FROM products;

-- Check nulls
SELECT * FROM sales WHERE customer_id IS NULL OR product_id IS NULL;

DELETE FROM sales WHERE customer_id IS NULL OR product_id IS NULL;

3. Business Analysis Queries

--Data Analysis & Business key problems & Answers

--My Analysis & Findings

--1Q.write a sql query to retrive all columns for sales made on '2022'
select *
from sales
where extract(year from order_date)=2022;

--2Q. write a sql query to retrieve what percentage of orders were completed vs cancelled?
select order_status,count(*) as total_orders,
round((count(*) *100.0/(select count(*)from sales)),2)as percentage
                        from sales
group by order_status;	

--3Q.write a sql query to retrive top 10 most sold products?
select p.product_name,sum(s.quantity)as units_sold
from sales s
join products p on s.product_id=p.product_id
group by p.product_name 
order by units_sold desc limit 10;

--4Q. write a sql query to retrieve which category contributes highest revenue?
select p.category,sum(s.quantity * p.price)as revenue
from sales s 
join products p on s.product_id=p.product_id
group by p.category 
order by revenue desc;

--5Q. write a sql query to retrieve which brand performs best in sales?
select brand,sum(quantity * price) as total_revenue
from sales s
join products p on s.product_id=p.product_id
group by brand 
order by total_revenue desc;


--6Q.write a sql query to retrieve which customers purchased most frequently?
select customer_id,count(order_id)as total_orders
from sales 
group by customer_id
order by total_orders desc limit 10;

--7Q. write a sql query to retrieve which orders were returned?
select order_id 
from shippings
where return_date is not null;

--8Q.write a sql query to retrieve best month of sales overall?
SELECT TO_CHAR(order_date,'YYYY-MM') AS month,
       SUM(quantity*price) AS revenue
FROM sales s
JOIN products p on s.product_id=p.product_id
GROUP BY month ORDER BY revenue DESC;

--9Q.write a sql query to retrieve state wise revenue performance?
select c.state,sum(s.quantity * p.price) as revenue
from sales s
join customers c on s.customer_id=c.customer_id
join products p on s.product_id=p.product_id
group by c.state 
order by revenue desc;

--10Q.write a sql query to retrieve unique customers purchased?
select count(distinct customer_id)as unique_customers 
from sales;

Conclusion
This Flipkart sales analysis  project  can be used to derive meaningful business insights using SQL. By working with five interconnected tables  Customers, Products, Sales, Payments, and Shipping i explored sales performance, customer behavior, order delivery patterns, and revenue contribution across multiple dimensions.

Throughout the analysis, multiple business questions were answered including sales trends, top-selling products, most valuable customers, payment preferences, delivery  and state-wise revenue performance. These insights not only reflect real-world analytical practices used by e-commerce companies but also help in supporting strategic decision-making like inventory planning, discount targeting, marketing segmentation, logistics partner evaluation, and customer retention programs.
		







