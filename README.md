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

SELECT distinct Product, price
FROM BIT_DB.JanSales
WHERE  price in (SELECT min(price) FROM BIT_DB.JanSales)

#5. What is the total revenue for each product sold in January?
SELECT sum(quantity)*price as revenue
,product
FROM BIT_DB.JanSales
GROUP BY product

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

#8 List all products sold in Los Angeles, and include how many of each were sold. 
SELECT distinct Product, sum(quantity)
FROM BIT_DB.FebSales
WHERE location like '%Los Angeles%'
GROUP BY Product;

Here I made tables of the "Lords and Ladies of Middle Earth":

CREATE TABLE Lords_of_Middle_Earth (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT, age INTEGER, race TEXT, skill TEXT);

INSERT INTO Lords_of_Middle_Earth VALUES (1, "Elrond", 8000, "elf", "Councler");
INSERT INTO Lords_of_Middle_Earth VALUES
(2, "Aragorn", 100, "dúnedain", "king");
INSERT INTO Lords_of_Middle_Earth VALUES
(3, "Samwise", 30, "hobbit", "gardner");
INSERT INTO Lords_of_Middle_Earth VALUES
(4, "Farimir", 50, "human", "duke");
INSERT INTO Lords_of_Middle_Earth VALUES
(5, "Gimli", 60, "dwarf", "soldier");
INSERT INTO Lords_of_Middle_Earth VALUES
(6, "Gandalf", 8000, "maiar", "wizard");
INSERT INTO Lords_of_Middle_Earth VALUES
(7, "Legolas", 3000, "elf", "archer");
INSERT INTO Lords_of_Middle_Earth VALUES
(8, "Pippin", 30, "hobbit", "gaurd");


CREATE TABLE Ladies_of_Middle_Earth (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT, age INTEGER, race TEXT, skill TEXT);

INSERT INTO Ladies_of_Middle_Earth VALUES
(1, "Arwen", 2700, "elf", "queen");
INSERT INTO Ladies_of_Middle_Earth VALUES
(2, "Eowyn", 40, "human", "dutchess");
INSERT INTO Ladies_of_Middle_Earth VALUES
(3, "Galadriel", 8000, "elf", "queen");



# For this project, I downloaded Spotify data from Kaggle.
# Then I created a table to insert Spotify data into.
# Finally, I performed analytics on the data using SQL. 

#Creating the table: 
CREATE TABLE BIT_DB.Spotifydata (
id integer PRIMARY KEY,
artist_name varchar NOT NULL,
track_name varchar NOT NULL,
track_id varchar NOT NULL,
popularity integer NOT NULL,
danceability decimal(4,3) NOT NULL,
energy decimal(4,3) NOT NULL,
key integer NOT NULL,
loudness decimal(5,3) NOT NULL,
mode integer NOT NULL,
speechiness decimal(5,4) NOT NULL,
acousticness decimal(6,5) NOT NULL,
instrumentalness text NOT NULL,
liveness decimal(5,4) NOT NULL,
valence decimal(4,3) NOT NULL,
tempo decimal(6,3) NOT NULL,
duration_ms integer NOT NULL,
time_signature integer NOT NULL 
)

#Then I inserted the Spotify Data .csv into the table.

#Next, I explored the data using the following SQL. 

#First I determined the avg popularity, danceability, and energy by artist and track. 
SELECT
artist_name
,track_name
,avg(popularity)
,avg(danceability)
,avg(energy)
FROM BIT_DB.spotifydata
GROUP BY artist_name, track_name

#Then I determined who the Top 10 artists are by popularity
SELECT track_name, artist_name, popularity
FROM BIT_DB.spotifydata
ORDER BY popularity desc
LIMIT 10
