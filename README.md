# Домашняя работа к занятию «SQL. Часть 2» - Боровик А. А.

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:

- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

Ответ:

```SQL
SELECT CONCAT(staff.last_name, ' ', staff.first_name) "staff name", city "store city", COUNT(customer_id) customers
FROM city c
INNER JOIN address a ON a.city_id = c.city_id
INNER JOIN store s ON s.address_id = a.address_id
LEFT JOIN staff ON s.store_id = staff.staff_id
LEFT JOIN customer ON customer.store_id = s.store_id
GROUP BY city, CONCAT(staff.last_name, ' ', staff.first_name)
HAVING customers > 300
;
```

![Задание 1](https://github.com/Lex-Chaos/SQL-2-hw/blob/master/img/SQL2_Task_1.png)

---

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

Ответ:

```SQL
SELECT COUNT(film_id)
FROM film
WHERE `length` > (SELECT AVG(`length`) FROM film)
;
```

![Задание 2](https://github.com/Lex-Chaos/SQL-2-hw/blob/master/img/SQL2_Task_2.png)

---

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

Ответ:

```SQL
SELECT paymonth, order_count
FROM (
SELECT MONTHNAME(payment_date) paymonth,
 sum(amount) summ,
 COUNT(payment_id) order_count
FROM payment p
GROUP BY paymonth
) AS summ
ORDER BY summ DESC
LIMIT 1
```

![Задание 3](https://github.com/Lex-Chaos/SQL-2-hw/blob/master/img/SQL2_Task_3.png)

---

### Задание 4*

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

Ответ:

```SQL
SELECT
 CONCAT(last_name, ' ', first_name) Staff,
 COUNT(rental_id) Sales,
 CASE
 	WHEN COUNT(rental_id) > 8000 THEN 'Да'
	WHEN COUNT(rental_id) < 8000 THEN 'Нет'
 END "Премия"
FROM staff s
INNER JOIN rental r ON r.staff_id = s.staff_id
GROUP BY Staff
```

![Задание 4](https://github.com/Lex-Chaos/SQL-2-hw/blob/master/img/SQL2_Task_4.png)

---

### Задание 5*

Найдите фильмы, которые ни разу не брали в аренду.

Ответ:

```SQL
SELECT f.title
FROM film f
LEFT JOIN inventory i ON i.film_id = f.film_id
LEFT JOIN rental r ON r.inventory_id = i.inventory_id
WHERE r.rental_id IS NULL
;
```

![Задание 5](https://github.com/Lex-Chaos/SQL-2-hw/blob/master/img/SQL2_Task_5.png)
