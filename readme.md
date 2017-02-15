Explorer Mode

How many users are there?
SELECT COUNT(*) FROM users;
50

What are the 5 most expensive items?
SELECT title
FROM items
ORDER BY price DESC
LIMIT 5;
title
Small Cotton Gloves
Small Wooden Computer
Awesome Granite Pants
Sleek Wooden Hat
Ergonomic Steel Car

What's the cheapest book? (Does that change for "category is exactly 'book'" versus "category contains 'book'"?)
SELECT title
FROM items
WHERE catagory LIKE 'book'
ORDER BY price
LIMIT 1
Ergonomic Granite Chair

Who lives at "6439 Zetta Hills, Willmouth, WY"? Do they have another address?
SELECT user_id
FROM addresses
WHERE
street = "6439 Zetta Hills";
40
SELECT first_name,last_name
FROM users
WHERE users.id = 40;
Corrine|Little

Do they have more than one address?
SELECT (*)
FROM addresses
WHERE user_id = 40;
43|40|6439 Zetta Hills|Willmouth|WY|15029
44|40|54369 Wolff Forges|Lake Bryon|CA|31587

Correct Virginie Mitchell's address to "New York, NY, 10108".
user_id = 39 (by manual search)
UPDATE addresses
SET city = 'New York',
state = 'NY',
zip = '10108'
WHERE user_id = 39;
41|39|12263 Jake Crossing|New York|NY|10108
42|39|83221 Mafalda Canyon|New York|NY|10108

How much would it cost to buy one of each tool
SELECT sum(price)
FROM items
WHERE category = 'Tools';
7,383   (via 'sum')

How many total items did we sell?
SELECT sum(quantity)
FROM orders
sum(quantity)
2126

#Note: I had to get help on this one !!!!
How much was spent on books?
SELECT sum(items.price * orders.quantity)
FROM orders
INNER JOIN items
ON orders.item_id = items.id
WHERE items.category LIKE '%book%';
sum(items.price * orders.quantity)
1081352

Simulate buying an item by inserting a User for yourself and an Order for that User.
INSERT INTO users
(first_name, last_name, email)
VALUES
('Rob', 'Taylor', 'epr1935@icloud.com')
51|Rob|Taylor|epr1935@icloud.com
INSERT INTO orders
(user_id, item_id, quantity, created_at)
VALUES
(511, 4, 1, '2015-02-09 00:40:31.307668' )
378|511|4|1|2015-02-09 00:40:31.307668

Adventure Mode
What item was ordered most often?
SELECT items.title,sum(orders.quantity)
FROM orders
INNER JOIN items
ON orders.item_id = items.id
GROUP BY orders.item_id
ORDER BY sum(orders.quantity) DESC
LIMIT 1;

Grossed the most money?
SELECT items.title,sum(orders.quantity * items.price)
FROM orders
INNER JOIN items
ON orders.item_id = items.id
GROUP BY orders.item_id
ORDER BY sum(orders.quantity) DESC
LIMIT 1;
Incredible Granite Car|525240

What user spent the most?
SELECT users.first_name,users.last_name,sum(orders.quantity * items.price) AS gross
FROM orders
INNER JOIN items
ON orders.item_id = items.id
INNER JOIN users
ON users.id = orders.user_id
GROUP BY orders.user_id
ORDER BY gross DESC
LIMIT 1;
Hassan|Runte|639386

What were the top 3 highest grossing categories?
SELECT items.category as cat
FROM orders
INNER JOIN items
ON orders.item_id = items.id
GROUP BY items.category
ORDER BY sum(orders.quantity * items.price) DESC
LIMIT 3;
Music, Sports & Clothing
Beauty, Toys & Sports
Sports

Epic Mode

Complete the exercises on sqlteaching.com as well and add a screenshot of the final screen showing all exercises completed to your README.
