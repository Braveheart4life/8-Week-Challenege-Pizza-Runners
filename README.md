# 8-Week-Challenege-Pizza-Runners

--1a) Create a new database called 'pizza_runner'
-- Connect to the 'master' database to run this snippet
USE master
GO
-- Create the new database if it does not exist already
IF NOT EXISTS (
    SELECT [name]
        FROM sys.databases
        WHERE [name] = N'pizza_runner
    '
)
CREATE DATABASE pizza_runner
GO

/*
SELECT * FROM customer_orders;
select * from pizza_names;
select * from pizza_recipes;
select * from pizza_toppings;
SELECT * FROM runners;
SELECT * FROM RUNNER_ORDERS;
*/

--1b) CREATE the first TABLE in the "pizza_runner" database called "pizza_names"
DROP TABLE IF EXISTS pizza_names;
CREATE TABLE pizza_names(
    "pizza_id" INTEGER, "pizza_name" TEXT
);

--1c) Insert values into the created columns of "pizza_id" and "pizza_name" as shown below:
INSERT INTO pizza_names
    ("pizza_id", "pizza_name")
VALUES 
    (1,'Meatlovers'),
    (2, 'Vegetarian');

--2a) CREATE second TABLE called "pizza_recipes" and INSERT VALUES INTO the table as shown:
DROP TABLE IF EXISTS pizza_recipes;
CREATE TABLE pizza_recipes( 
    "pizza_id" INTEGER, "toppings" TEXT
);
--2b)     
INSERT INTO pizza_recipes
    ("pizza_id", "toppings")
VALUES
    (1, '1, 2, 3, 4, 5, 6, 8, 10'),
    (2, '4, 6, 7, 9, 11, 12');

--3a) CREATE third TABLE called "pizza_toppings" and INSERT VALUES INTO the TABLE as shown below:
DROP TABLE IF EXISTS pizza_toppings;
CREATE TABLE pizza_toppings(
    "topping_id" INTEGER, "topping_name" TEXT
);
--3b)
INSERT INTO pizza_toppings
    ("topping_id", "topping_name")
VALUES
    (1, 'Bacon'),
    (2, 'BBQ Sauce'),
    (3, 'Beef'),
    (4, 'Cheese'),
    (5, 'Chicken'),
    (6, 'Mushrooms'),
    (7, 'Onions'),
    (8, 'Pepperoni'),
    (9, 'Peppers'),
    (10, 'Salami'),
    (11, 'Tomatoes'),
    (12, 'Tomato Sauce');

--4a) CREATE TABLE "runners" and INSERT VALUES INTO it as shown:
DROP TABLE IF EXISTS runners;
CREATE TABLE runners(
    "runners_id" INTEGER, "registration_date" DATE
);
--4b) 
INSERT INTO runners
    ("runners_id", "registration_date")
VALUES
    (1, '2021-01-01'),
    (2, '2021-01-03'),
    (3, '2021-01-08'),
    (4, '2021-01-15');

--5a) CREATE TABLE customer_orders and INSERT VALUES INTO it as shown below:
DROP TABLE IF EXISTS customer_orders;
CREATE TABLE customer_orders(
    "order_id" INTEGER, 
    "customer_id" INTEGER, 
    "pizza_id" INTEGER, 
    "exclusions" VARCHAR(4),
    "extras" VARCHAR(4),
    "order_time" DATETIME
    );

INSERT INTO customer_orders
  ("order_id", "customer_id", "pizza_id", "exclusions", "extras", "order_time")
VALUES
  ('1', '101', '1', '', '', '2020-01-01 18:05:02'),
  ('2', '101', '1', '', '', '2020-01-01 19:00:52'),
  ('3', '102', '1', '', '', '2020-01-02 23:51:23'),
  ('3', '102', '2', '', NULL, '2020-01-02 23:51:23'),
  ('4', '103', '1', '4', '', '2020-01-04 13:23:46'),
  ('4', '103', '1', '4', '', '2020-01-04 13:23:46'),
  ('4', '103', '2', '4', '', '2020-01-04 13:23:46'),
  ('5', '104', '1', 'null', '1', '2020-01-08 21:00:29'),
  ('6', '101', '2', 'null', 'null', '2020-01-08 21:03:13'),
  ('7', '105', '2', 'null', '1', '2020-01-08 21:20:29'),
  ('8', '102', '1', 'null', 'null', '2020-01-09 23:54:33'),
  ('9', '103', '1', '4', '1, 5', '2020-01-10 11:22:59'),
  ('10', '104', '1', 'null', 'null', '2020-01-11 18:34:49'),
  ('10', '104', '1', '2, 6', '1, 4', '2020-01-11 18:34:49');

--6a) CREATE TABLE runner_orders and INSERT VALUES INTO it as shown below:
DROP TABLE IF EXISTS runner_orders;
CREATE TABLE runner_orders(
  "order_id" INTEGER,
  "runner_id" INTEGER,
  "pickup_time" VARCHAR(19),
  "distance" VARCHAR(7),
  "duration" VARCHAR(10),
  "cancellation" VARCHAR(23)
);

INSERT INTO runner_orders
  ("order_id", "runner_id", "pickup_time", "distance", "duration", "cancellation")
VALUES
  ('1', '1', '2020-01-01 18:15:34', '20km', '32 minutes', ''),
  ('2', '1', '2020-01-01 19:10:54', '20km', '27 minutes', ''),
  ('3', '1', '2020-01-03 00:12:37', '13.4km', '20 mins', NULL),
  ('4', '2', '2020-01-04 13:53:03', '23.4', '40', NULL),
  ('5', '3', '2020-01-08 21:10:57', '10', '15', NULL),
  ('6', '3', 'null', 'null', 'null', 'Restaurant Cancellation'),
  ('7', '2', '2020-01-08 21:30:45', '25km', '25mins', 'null'),
  ('8', '2', '2020-01-10 00:15:02', '23.4 km', '15 minute', 'null'),
  ('9', '2', 'null', 'null', 'null', 'Customer Cancellation'),
  ('10', '1', '2020-01-11 18:50:20', '10km', '10minutes', 'null');


--7a) in "customer_orders" We are seperating the "order_time" column into two columns that differentiate the 'time of order' and the 'date of order' as follows:

SELECT CAST(order_time AS TIME) FROM customer_orders; -- shows only the time of order
SELECT CAST(order_time AS DATE) FROM customer_orders;-- shows only the date of orders

--7b) Let's add the two new columns by ALTER TABLE and the we can UPDATE the table with the preferred format and data as shown:
ALTER TABLE customer_orders -- we added a new column called "time_of_order"
ADD time_of_order TIME 

ALTER TABLE customer_orders -- we added a new column called "date_of_order"
ADD date_of_order DATE
--7c) UPDATE statements: Inputing the data in the previous CAST Statements from 7a, into the new created columns
UPDATE customer_orders
SET time_of_order = CAST(order_time AS TIME)

UPDATE customer_orders
SET date_of_order = CAST(order_time AS DATE)


--8a) cleaning data: UPDATE (runner_order) by replacing "distance" and "duration" columns with their numerical values only as shown:

-- These queries extract numbers only from "duration" and "distance" columns:
SELECT SUBSTRING(duration, PATINDEX('%[0-9]%', duration), PATINDEX('%[0-9][^0-9]%', duration + 't')-PATINDEX('%[0-9]%', duration) + 1) AS Number
FROM runner_orders

SELECT SUBSTRING(distance, PATINDEX('%[0-9]%', distance), PATINDEX('%[0-9][^0-9]%', distance + 't')-PATINDEX('%[0-9]%', distance) + 1) AS Number
FROM runner_orders

-- Queries to ALTER TABLE and replace data in desired columns:
UPDATE runner_orders-- for [distance] column
SET distance = SUBSTRING(distance, PATINDEX('%[0-9]%', distance), PATINDEX('%[0-9][^0-9]%', distance + 't')-PATINDEX('%[0-9]%', distance) + 1) 

UPDATE runner_orders-- for [duration] column
SET duration = SUBSTRING(duration, PATINDEX('%[0-9]%', duration), PATINDEX('%[0-9][^0-9]%', duration + 't')-PATINDEX('%[0-9]%', duration) + 1) 

--8b) replacing "null" values in 'pick_up_time' column and 'cancellation' column with ' ' (empty)
UPDATE runner_orders
SET pickup_time = ''
WHERE pickup_time = 'null'

UPDATE runner_orders
SET cancellation = ''
WHERE cancellation = 'null'

UPDATE runner_orders
SET cancellation = ''
WHERE cancellation IS NULL 

SELECT * FROM RUNNER_ORDERS;

--8c) replacing "null" values in 'extras' column and 'exclusion' column with ' ' (empty)

UPDATE customer_orders
SET exclusions = ''
WHERE exclusions = 'null'

UPDATE customer_orders
SET extras = ''
WHERE extras = 'null'

UPDATE customer_orders
SET extras = ''
WHERE extras IS NULL

UPDATE customer_orders
SET exclusions = ''
WHERE exclusions IS NULL

--8d) checking for duplicates in the customer_order table, we use ROW_NUMBER to compare simililarities in the columns within the PARTITION BY as follows:
select 
    *,
    ROW_NUMBER() OVER(
        PARTITION BY   
            order_id,
            customer_id,
            pizza_id,
            exclusions,
            extras,
            order_time
        ORDER BY order_id       
    ) as duplicate_2
from customer_orders

--USE CTE to show duplicates
with duplicate_check as (
    select 
    *,
    ROW_NUMBER() OVER(
        PARTITION BY   
            order_id,
            customer_id,
            pizza_id,
            exclusions,
            extras,
            order_time
        ORDER BY order_id       
    ) as duplicate_2
from customer_orders
)
select * 
from duplicate_check
where Duplicate_2 > 1

-- DELETE duplicate rows from "customer_orders" using the above CTE as shown:

with duplicate_check as (
    select 
    *,
    ROW_NUMBER() OVER(
        PARTITION BY   
            order_id,
            customer_id,
            pizza_id,
            exclusions,
            extras,
            order_time
        ORDER BY order_id       
    ) as Duplicate_2
from customer_orders
)
DELETE 
FROM duplicate_check
WHERE Duplicate_2 > 1

--------------------------------------------------------------------------------
---------------------------------QUESTIONS--------------------------------------
--------------------------------------------------------------------------------
--a) How many pizzas were ordered?
SELECT SUM(pizza_id) as [total ordered pizza]
FROM customer_orders

--b) How many unique customer orders were made?
SELECT COUNT(DISTINCT order_id) AS [no of Unique orders]
FROM customer_orders

--c) How many successful orders were delivered by each runner?
SELECT 
    DISTINCT runner_id,
    COUNT(duration) AS [no of deliveries]
FROM runner_orders
WHERE cancellation = ''
GROUP BY runner_id

--d) How many of each type of pizza was delivered?
SELECT 
    --DISTINCT a.pizza_id,
    b.pizza_name,
    COUNT(a.pizza_id) AS [no delivered]
FROM customer_orders a 
JOIN pizza_names b ON
a.pizza_id = b.pizza_id
GROUP BY a.pizza_id, b.pizza_name --(I HAD TO CONVERT DATA TYPE FROM "TEXT" TO "NVACHAR(50)" BEFORE I COULD JOIN THE TABLES)

--e) How many Vegetarian and Meatlovers were ordered by each customer?
SELECT 
    DISTINCT A.customer_id,
    B.pizza_name,
    COUNT(A.pizza_id) AS order_count
FROM customer_orders A
JOIN pizza_names B ON
A.pizza_id = B.pizza_id
WHERE A.pizza_id = 1
GROUP BY A.customer_id, B.pizza_name
UNION
SELECT 
    DISTINCT A.customer_id,
    B.pizza_name,
    COUNT(A.pizza_id) AS order_count
FROM customer_orders A
JOIN pizza_names B ON 
A.pizza_id = B.pizza_id
WHERE A.pizza_id = 2
GROUP BY A.customer_id, B.pizza_name

--f) What was the maximum number of pizzas delivered in a single order?
SELECT TOP 1
   max(pizza_id) as [no of pizza delivered]
FROM customer_orders

--g) For each customer, how many delivered pizzas had at least 1 change and how many had no changes?
select 
    distinct customer_id,
    count(customer_id) as count
from customer_orders
where extras is not null OR
    exclusions is not null 
group by customer_id

--h) How many pizzas were delivered that had both exclusions and extras?
SELECT 
    COUNT(a.customer_id) AS [Count with extras and exclusions]
FROM customer_orders a 
JOIN RUNNER_ORDERS b ON
    a.order_id = b.order_id
WHERE 
    a.exclusions LIKE '%' AND 
    a.extras LIKE '%' AND
    b.cancellation  IS NULL 

/* THIS IS HOW YOU SET EMPTY CELLS TO NULL VALUES IF NECESSARY:
    UPDATE customer_orders 
    SET exclusions = NULLIF(exclusions, ''),
    extras = NULLIF(extras, '')

    UPDATE runner_orders 
    SET cancellation = NULLIF(cancellation, '')
*/

--I) What was the total volume of pizzas ordered for each hour of the day?
SELECT 
    order_time as [Time],
    count(order_id) AS [No of orders],
    CASE 
        WHEN order_time = '11%' THEN '11'
        end
FROM customer_orders
GROUP BY order_time
ORDER BY 1 ASC;

SELECT 
    time_of_order as [Time],
    count(order_id) AS [No of orders]
FROM customer_orders
GROUP BY time_of_order
ORDER BY 1 ASC;

--J) What was the volume of orders for each day of the week?
SELECT date_of_order, COUNT(customer_id) as OrderCount
FROM customer_orders
GROUP BY date_of_order
ORDER BY date_of_order;

-----------------------------------------------------------
-----------------------SECTION B---------------------------
-------------Runner and Customer Experience----------------
-----------------------------------------------------------
--1) How many runners signed up for each 1 week period? (i.e. week starts 2021-01-01)
SELECT 
    CAST(pickup_time as date) [date], 
    COUNT (DISTINCT runner_id) as [no of runners]
FROM RUNNER_ORDERS
GROUP BY pickup_time
ORDER BY pickup_time
OFFSET 1 rows
FETCH NEXT 9 ROWS ONLY;

--2) What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pickup the order?
SELECT AVG(cast(duration as int)) AS [average delivery time in minutes]
FROM RUNNER_ORDERS;

--3) Is there any relationship between the number of pizzas and how long the order takes to prepare?
SELECT 
    COUNT(C.pizza_id) AS [NO OF PIZZAS],
    --C.order_time,
    --R.pickup_time,
    CAST((R.pickup_time  - C.order_time) AS time) AS [Time Diff] 
FROM customer_orderS C 
JOIN runner_orders R
    ON C.order_id = R.order_id
WHERE R.pickup_time NOT LIKE '_'
GROUP BY C.pizza_id, C.order_time, R.pickup_time

--FROM THE RESULTS OBTAINED, THERE IS NO RELATIONSHIP BETWEEN THE TIME TO PREPARE ORDERS AND THE NO OF ORDERS MADE

--4) What was the average distance travelled for each customer?
SELECT 
    C.customer_id, 
    AVG(CAST(R.distance AS int)) AS [AVERAGE DISTANCE COVERED BY RUNNERS]
FROM customer_orders C 
JOIN runner_orders R 
    ON C.order_id = R.order_id
GROUP BY C.customer_id

--5) What was the difference between the longest and shortest delivery times for all orders?
SELECT 
    (MAX(CAST(duration AS int)) - MIN(CAST(duration AS int))) as [difference between longest and shortest delivery in minutes]
FROM RUNNER_ORDERS
WHERE CAST(duration AS int) > 1 
AND CAST(distance AS int) > 1

--6) What was the average speed for each runner for each delivery and do you notice any trend for these values?
SELECT 
    DISTINCT R.runner_id,
    R.order_id,
    ROUND((CAST(R.distance as int)/CAST(duration AS int)),3)  AS [Speed(km/hr)]
FROM runner_orders R 
LEFT OUTER JOIN customer_orders C 
    ON C.order_id = R.order_id
WHERE CAST(duration AS int) > 1 
AND CAST(distance AS int) > 1

--7) What is the successful delivery percentage for each runner?
--total no of successful deliveries by runners
SELECT 
    runner_id,
    COUNT(runner_id) AS [NO delivered]
FROM runner_orders
WHERE cancellation IS NULL
GROUP BY runner_id 
--total number of scheduled deliveries to runners
SELECT 
    runner_id,
    COUNT(runner_id) AS [NO of assigned deliveries]
FROM runner_orders
GROUP BY runner_id 

---create view to store Query, then i can query that original query:

CREATE VIEW Scheduled_Deliveries AS
SELECT 
    runner_id,
    COUNT(runner_id) AS [NO of assigned deliveries]
FROM runner_orders
GROUP BY runner_id 

SELECT runner_id,
    COUNT(runner_id) AS [SCHEDULED deliveries],
    (SELECT
        COUNT(runner_id) OVER (PARTITION BY runner_id)
        FROM runner_orders 
        WHERE cancellation IS NULL
       ) AS [successful deliveries]
FROM RUNNER_ORDERS
GROUP BY runner_id

--------------------------------------------------------------------------------------
------------------------SECTION C: Ingredient Optimization----------------------------
--------------------------------------------------------------------------------------
--1) What are the standard ingredients for each pizza?
SELECT 
    cast(pn.pizza_name as text),
    cast(pt.topping_name as text)
FROM pizza_names pn
JOIN pizza_recipes pr ON
    pn.pizza_id = pr.pizza_id
JOIN pizza_toppings pt ON
pr.toppings = pt.topping_id

SELECT 
    *, 
    SUBSTRING(toppings, 1, 1),
    SUBSTRING(toppings, 4, 1),
    SUBSTRING(toppings, 7, 1),
    SUBSTRING(toppings, 10, 1),
    SUBSTRING(toppings, 13, 2),
    SUBSTRING(toppings, 16,2),
    SUBSTRING(toppings, 19, 2),
    SUBSTRING(toppings, 22, 2)
FROM pizza_recipes

SELECT pizza_id, toppings, VALUE  
FROM pizza_recipes
    CROSS APPLY string_split(toppings, ',');


SELECT * FROM customer_orders;
select * from pizza_names;
select * from pizza_recipes;
select * from pizza_toppings;
SELECT * FROM runners;
SELECT * FROM RUNNER_ORDERS;


