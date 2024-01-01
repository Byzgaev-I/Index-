# Домашнее задание к занятию "`Индексы`" - `Бызгаев Александр`

---

### Задание 1

Напишите запрос к учебной базе данных, который вернёт процентное отношение общего размера всех индексов к общему размеру всех таблиц.

### Решение:

```
select sum(data_length) as SUM_Data_Length, sum(index_length) as SUM_Index_Length, sum(index_length)*100.0/sum(data_length) as Persentage_ratio
from information_schema.tables
where table_schema='sakila' and data_length is not null;
```

![image](https://github.com/Byzgaev-I/Index/blob/main/Index%20-1.png)

---

### Задание 2

Выполните explain analyze следующего запроса:

```
select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
```

### Решение:

Вывод запроса:

![image](https://github.com/Byzgaev-I/Index/blob/main/Index%20-2.png)

Я могу использовать EXPLAIN ANALYZE для нашего запроса:

```
EXPLAIN ANALYZE
SELECT DISTINCT CONCAT(c.last_name, ' ', c.first_name),
       SUM(p.amount) OVER (PARTITION BY c.customer_id, f.title)
FROM payment p
JOIN rental r ON p.payment_date = r.rental_date
JOIN customer c ON r.customer_id = c.customer_id
JOIN inventory i ON i.inventory_id = r.inventory_id
JOIN film f ON f.film_id = i.film_id
WHERE DATE(p.payment_date) = '2005-07-30';

```
После выполнения этого запроса в вашей среде вы увидите подробный план выполнения и статистику, которая поможет идентифицировать узкие места.  
Потенциальные узкие места могут включать:  
Использование функции 'DATE()' на p.payment_date, что может препятствовать использованию индексов.  
Возможное отсутствие индексов на ключевых столбцах для соединений и фильтрации.  


---

