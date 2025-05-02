Задание 1
Напишите запрос к учебной базе данных, который вернёт процентное отношение общего размера всех индексов к общему размеру всех таблиц.


Решение1


![img](https://github.com/Furious992/HW-12-5/blob/main/1.png)

Задание 2
Выполните explain analyze следующего запроса:

select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
перечислите узкие места;
оптимизируйте запрос: внесите корректировки по использованию операторов, при необходимости добавьте индексы.


Решение 2



EXPLAIN ANALYZE
SELECT DISTINCT CONCAT(c.last_name, ' ', c.first_name), 
       SUM(p.amount) OVER (PARTITION BY c.customer_id, f.title)
FROM payment p, rental r, customer c, inventory i, film f
WHERE DATE(p.payment_date) = '2005-07-30' 
  AND p.payment_date = r.rental_date 
  AND r.customer_id = c.customer_id 
  AND i.inventory_id = r.inventory_id;


  Оптимизированный
  
  EXPLAIN ANALYZE
SELECT 
    CONCAT(c.last_name, ' ', c.first_name) AS customer_name,
    SUM(p.amount) AS total_payment
FROM payment p
JOIN rental r ON p.rental_id = r.rental_id
JOIN customer c ON r.customer_id = c.customer_id
JOIN inventory i ON r.inventory_id = i.inventory_id
WHERE p.payment_date >= '2005-07-30' 
  AND p.payment_date < '2005-07-31'
GROUP BY c.customer_id, c.last_name, c.first_name;
  
