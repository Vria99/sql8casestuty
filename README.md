All the data and questions that I've answered in this markdown can be found at Dannys Website.
And all my solutions have been executed at DB Fiddle using POSTGRE SQL V 13 as given on the website

QUESTION NO 1. What is the total amount each customer spent at the restaurant?

```
SELECT 
customer_id,
SUM(price) AS total_amount
FROM SALES as S
INNER JOIN menu as M on S.product_id = M.product_id
GROUP BY 
    customer_id
```
![sqldanny 1](https://github.com/user-attachments/assets/23bcbc4a-afd4-441d-9218-d2130c86bfe3)

QUESTION NO2. How many days has each customer visited the restaurant?
```

SELECT customer_id, 
COUNT(DISTINCT order_date)
FROM SALES
GROUP BY customer_id

```
![sqldanny2](https://github.com/user-attachments/assets/54b50aba-e852-4170-bfe1-dd57cf9f45d1)

QUESTION NO3. What was the first item from the menu purchased by each customer?

```

SELECT DISTINCT customer_id,
                product_name AS first_item_purchased
FROM (
    SELECT customer_id,
           product_name,
           DENSE_RANK() OVER (PARTITION BY customer_id ORDER BY order_date) AS dn
    FROM SALES AS S 
  INNER JOIN menu as M on S.product_id = M.product_id
) temp
WHERE dn = 1;

```
![SQldanny 3](https://github.com/user-attachments/assets/532288b4-eb77-4882-84e8-c78b7b20adf8)

QUESTION NO4. What is the most purchased item on the menu and how many times was it purchased by all customers?

```
SELECT product_name,
COUNT ( product_name ) as ORDERS
FROM SALES as S
INNER JOIN menu as M on S.product_id = M.product_id
GROUP BY product_name
ORDER BY ORDERS DESC
limit 1

```
