# Common Table Expression

## Description

* Calculate the total sales, ``total_sales``, for each customer.
* Print the result contains info of ``customer_name`` and ``total_sales``.
* Arrange the result in descending order of ``total_sales``.

## Notes

* The total sales for each customer is calculated by sum up all the sales by customer.
* To print out ``customer_name`` and ``total_sales``, we need info from two tables ``customers`` and ``sales``.
* The first step of summing up the sales can be stored in a CTE, then it can be referred in later use.

## Solution

```sql
WITH total_sales_by_customer AS (
    SELECT customer_id, sum(amount) AS total_sales
    FROM sales
    GROUP BY customer_id
)
SELECT customer_name, total_sales
FROM customers
JOIN total_sales_by_customer on total_sales_by_customer.customer_id = customers.customer_id
ORDER BY total_sales_by_customer.total_sales DESC;
```

## Appendix

```sql
create table sales
(
    transaction_id serial
        constraint sales_pk
            primary key,
    customer_id    integer,
    product_name   text,
    category_name  text,
    amount         integer
);
 
alter table sales
    owner to postgres;
 
INSERT INTO public.sales (transaction_id, customer_id, product_name, category_name, amount) VALUES (1, 1, 'Product A', 'Category 1', 10);
INSERT INTO public.sales (transaction_id, customer_id, product_name, category_name, amount) VALUES (2, 2, 'Product B', 'Category 1', 15);
INSERT INTO public.sales (transaction_id, customer_id, product_name, category_name, amount) VALUES (3, 3, 'Product C', 'Category 2', 20);
INSERT INTO public.sales (transaction_id, customer_id, product_name, category_name, amount) VALUES (4, 4, 'Product D', 'Category 2', 12);
INSERT INTO public.sales (transaction_id, customer_id, product_name, category_name, amount) VALUES (5, 5, 'Product E', 'Category 3', 7);
INSERT INTO public.sales (transaction_id, customer_id, product_name, category_name, amount) VALUES (6, 1, 'Product F', 'Category 3', 5);
INSERT INTO public.sales (transaction_id, customer_id, product_name, category_name, amount) VALUES (7, 2, 'Product G', 'Category 4', 18);
INSERT INTO public.sales (transaction_id, customer_id, product_name, category_name, amount) VALUES (8, 3, 'Product H', 'Category 4', 22);
INSERT INTO public.sales (transaction_id, customer_id, product_name, category_name, amount) VALUES (9, 4, 'Product I', 'Category 5', 12);
INSERT INTO public.sales (transaction_id, customer_id, product_name, category_name, amount) VALUES (10, 5, 'Product J', 'Ctegory 5', 8);
 
 
create table customers
(
    customer_id   serial
        constraint customers_pk
            primary key,
    customer_name text
);
 
alter table customers
    owner to postgres;
 
INSERT INTO public.customers (customer_id, customer_name) VALUES (1, 'Customer A');
INSERT INTO public.customers (customer_id, customer_name) VALUES (2, 'Customer B');
INSERT INTO public.customers (customer_id, customer_name) VALUES (3, 'Customer C');
INSERT INTO public.customers (customer_id, customer_name) VALUES (4, 'Customer D');
INSERT INTO public.customers (customer_id, customer_name) VALUES (5, 'Customer E');
```
