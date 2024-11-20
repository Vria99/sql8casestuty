All the data and questions that I've answered in this markdown can be found at Dannys Website. (https://8weeksqlchallenge.com/case-study-1/)
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

5.  Which item was the most popular for each customer?

```
WITH CTE AS (
	SELECT 
	customer_id,
    COUNT(order_date) as orders,
    product_name,
   DENSE_RANK() OVER (PARTITION BY customer_id ORDER BY COUNT(order_date) DESC) AS position
     FROM SALES AS S 
  INNER JOIN menu as M on S.product_id = M.product_id
    GROUP BY M.product_name, customer_id
) 
SELECT 
*
FROM CTE 
WHERE position = 1

```
6. Which item was purchased first by the customer after they became a member?

```

WITH CTE AS (
    SELECT S.customer_id, order_date, product_name, 
    RANK() OVER (PARTITION BY S.customer_id ORDER BY order_date) AS rank
    FROM Sales as S
    INNER JOIN members as Mem on S.customer_id = Mem.customer_id
      INNER JOIN menu as M on S.product_id = M.product_id
    WHERE order_date >= join_date
)
SELECT 
customer_id, order_date, product_name
FROM CTE
WHERE rank = 1

```
7. Which item was purchased just before the customer became a member?
```
WITH CTE AS (
    SELECT S.customer_id,
    order_date,
    product_name, 
    RANK() OVER (PARTITION BY S.customer_id ORDER BY order_date DESC) AS rank
    FROM Sales as S
    INNER JOIN members as Mem on S.customer_id = Mem.customer_id
      INNER JOIN menu as M on S.product_id = M.product_id
    WHERE order_date < join_date
)
SELECT customer_id, order_date, product_name
FROM CTE
WHERE rank = 1

```
8. What is the total items and amount spent for each member before they became a member?```
```
SELECT S.customer_id,
SUM(price) AS total_amount_spent,
COUNT(order_date) AS times_purchased
FROM Sales as S
INNER JOIN members as Mem on S.customer_id = Mem.customer_id
INNER JOIN menu as M on S.product_id = M.product_id
WHERE order_date < join_date
GROUP BY S.customer_id

```
9.  If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
```


```
10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?
```
