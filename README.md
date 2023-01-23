# SQL-
SQL Portfolio

Here I created a Fantasy Shop Items List.

/** Items list: 
potions (5)
swords (3)
sheilds (3)
axes (2)
bows (2)
**/

CREATE TABLE items (id INTEGER PRIMARY KEY, name TEXT, quantity INTEGER );

INSERT INTO items VALUES (1, "potions", 5);
INSERT INTO items VALUES (2, "swords", 3);
INSERT INTO items VALUES (3, "sheilds", 3);
INSERT INTO items VALUES (4, "axes", 2);
INSERT INTO items VALUES (5, "bows", 2);

SELECT * FROM items;

Company questions answered:

#1. How many orders were placed in January? 
SELECT COUNT(orderid)
FROM BIT_DB.JanSales
WHERE length(orderid) = 6 
AND orderid <> 'Order ID'

#2. How many of those orders were for an iPhone? 
SELECT COUNT(orderid)
FROM BIT_DB.JanSales
WHERE Product='iPhone'
AND length(orderid) = 6 
AND orderid <> 'Order ID'

#3. Select the customer account numbers for all the orders that were placed in February. 
SELECT distinct acctnum
FROM BIT_DB.customers cust

INNER JOIN BIT_DB.FebSales Feb
ON cust.order_id=FEB.orderid
WHERE length(orderid) = 6 
AND orderid <> 'Order ID'

#4. Which product was the cheapest one sold in January, and what was the price?
# THERE ARE MULTIPLE CORRECT ANSWERS LISTED BELOW 

###### QUESTION 4 ANSWER #1 ######
SELECT distinct Product, price
FROM BIT_DB.JanSales
WHERE  price in (SELECT min(price) FROM BIT_DB.JanSales)

####### QUESTION 4 ANSWER #2 ######
SELECT distinct product, price 
FROM BIT_DB.JanSales 
ORDER BY price ASC LIMIT 1

####### QUESTION 4 ANSWER #3 ######
SELECT distinct product, MIN(price) 
FROM BIT_DB.JanSales Jan 
GROUP BY product, price 
ORDER BY price ASC LIMIT 1
#PLEASE NOTE: Did do use Option 3, but forgot the GROUP BY?
#If so, please see the Question 4 notes below the Answers section for an explanation.

#######  QUESTION 4 ANSWER #4 ######
SELECT product, min(price) 
FROM BIT_DB.JanSales Jan 
GROUP BY product, price 
ORDER BY price ASC
LIMIT 1

#5. What is the total revenue for each product sold in January?
SELECT sum(quantity)*price as revenue
,product
FROM BIT_DB.JanSales
GROUP BY product
#PLEASE NOTE: Are you unsure why we use sum(quantity)*price here? See Question 5 notes below the answer section for an explanation.

#6. Which products were sold in February at 548 Lincoln St, Seattle, WA 98101, how many of each were sold, and what was the total revenue?
select 
sum(Quantity), 
product, 
sum(quantity)*price as revenue
FROM BIT_DB.FebSales 
WHERE location = '548 Lincoln St, Seattle, WA 98101'
GROUP BY product

#7. How many customers ordered more than 2 products at a time, and what was the average amount spent for those customers? 
select 
count(distinct cust.acctnum), 
avg(quantity*price)
FROM BIT_DB.FebSales Feb
LEFT JOIN BIT_DB.customers cust
ON FEB.orderid=cust.order_id
WHERE Feb.Quantity>2
AND length(orderid) = 6 
AND orderid <> 'Order ID'
