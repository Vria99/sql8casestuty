All the data and questions that I've answered in this markdown can be found at Dannys Website.
And all my solutions have been executed at DB Fiddle using POSTGRE SQL V 13 as given on the website

To create a databsae to oversee all potential answers I have joint all 3 atables together.

WITH CTE as (
SELECT s.*,m.join_date,men.product_name,men.price
FROM dannys_diner.sales s 
FULL OUTER JOIN dannys_diner.members m ON s.customer_id = m.customer_id
FULL OUTER JOIN dannys_diner.menu men ON s.product_id = men.product_id
ORDER BY s.customer_id,order_date)
SELECT * FROM cte;
