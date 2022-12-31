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
