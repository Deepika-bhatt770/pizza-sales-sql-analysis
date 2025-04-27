# pizza-sales-sql-analysis



create database dominous;
create table orders (
order_id int primary key,
order_date date not null,
order_time time not null
);

select * from orders_details;
create table orders_details (
order_details_id int not null primary key,
order_id int not null,
pizza_id varchar(50),
quantity int not null
);


/*	Question Set 1 - Easy */


#retrieve the total number of order placed?

select count(order_id) as total_orders
from orders;


-- Calculate the total revenue generated from pizza sales.-- 
SELECT 
    round(sum(orders_details.quantity * pizzas.price),2) AS total_sales
FROM
    orders_details
        JOIN
    pizzas ON orders_details.pizza_id = pizzas.pizza_id;
    
    
    # Identify the highest-priced pizza.
SELECT 
    pizza_types.name, pizzas.price AS a
FROM
    pizzas
        JOIN
    pizza_types ON pizzas.pizza_type_id = pizza_types.pizza_type_id
ORDER BY a DESC
LIMIT 1;


-- Identify the most common pizza size ordered.
SELECT 
    COUNT(orders_details.quantity) AS total_orders, size
FROM
    orders_details
        JOIN
    pizzas ON orders_details.pizza_id = pizzas.pizza_id
GROUP BY size
ORDER BY total_orders DESC;

  
list the top 5 most ordered pizzas  along with their quantaty

SELECT 
    pt.name, COUNT(od.quantity) AS total_count
FROM
    pizzas AS p
        JOIN
    orders_details AS od ON od.pizza_id = p.pizza_id
        JOIN
    pizza_types AS pt ON p.pizza_type_id = pt.pizza_type_id
GROUP BY pt.name
ORDER BY total_count DESC
LIMIT 1;


* Question Set 2 - Moderate */

  
#joins the tables and found out the total quantity of each pizza category orderred

SELECT 
    pt.category, sum(od.quantity) AS total_quantity
FROM
    pizzas AS p
        JOIN
    pizza_types AS pt ON p.pizza_type_id = pt.pizza_type_id
        JOIN
    orders_details AS od ON p.pizza_id = od.pizza_id
GROUP BY pt.category
ORDER BY total_quantity DESC;



#Determine the distribution of orders by hour of the day.

select hour(order_time) as hours,count(order_id) as total_quantity
from orders  
group by hours
order by total_quantity desc;




-- Join relevant tables to find the category-wise distribution of pizzas.

select category,count(pizza_type_id) as total_count
 from pizza_types
 group by category
 order by total_count desc;


 
 -- Determine the top 3 most ordered pizza types based on revenue.\


SELECT 
    pt.name AS name_, SUM(price * quantity) AS revenue
FROM
    pizzas AS p
        JOIN
    orders_details AS od ON p.pizza_id = od.pizza_id
        JOIN
    pizza_types AS pt ON p.pizza_type_id = pt.pizza_type_id
GROUP BY name_
ORDER BY revenue DESC
LIMIT 3;



 /* Question Set 3 - Advance */

-- Calculate the percentage contribution of each pizza type to total revenue.
SELECT 
    pt.name,
    SUM(price * quantity) AS total_revenue,
    (SUM(price * quantity) / (SELECT SUM(price * quantity) as percentage FROM orders_details)) * 100 AS percentage_contribution
FROM pizzas as p
join pizza_types as pt on p.pizza_type_id=pt.pizza_type_id
join orders_details as od on od.pizza_id=p.pizza_id
GROUP BY pt.name
ORDER BY percentage_contribution DESC;
 
 
 
 -- Group the orders by date and 
-- calculate the average number of pizzas ordered per day.
SELECT 
    o.order_date AS num_day, AVG(od.quantity) AS av_quanity
FROM
    orders AS o
        JOIN
    orders_details AS od ON o.order_id = od.order_id
GROUP BY num_day
ORDER BY num_day DESC;
