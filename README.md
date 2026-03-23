# SQL-Academy
тренажер SQL-Academy https://sql-academy.org/ru/trainer
---
---
#### ✅***Задание №1:*** 
**Вывести имена всех людей, которые есть в базе данных авиакомпаний. Поля в результирующей таблице: name**

```SQL
SELECT name
FROM passenger;
```

---
#### ✅***Задание №2:*** 
**Вывести названия всеx авиакомпаний. Поля в результирующей таблице: name**

```SQL
SELECT name
FROM company;
```

---
#### ✅***Задание №3:*** 
**Вывести все рейсы, совершенные из Москвы. Поля в результирующей таблице:** \*

```SQL
SELECT *
FROM trip
WHERE town_from = 'Moscow';
```

---
#### ✅***Задание №4:*** 
**Вывести имена людей, которые заканчиваются на "man". Поля в результирующей таблице: name** 

```SQL
SELECT name
FROM Passenger
WHERE name regexp 'man$';
```

---
#### ✅***Задание №5:*** 
**Вывести количество рейсов, совершенных на TU-134. Используйте конструкцию "as count" для агрегатной функции подсчета количества рейсов. Это необходимо для корректной проверки. Поля в результирующей таблице: count** 

```SQL
SELECT count(id) as count
FROM Trip
WHERE plane = 'TU-134';
```

---
#### ✅***Задание №6:*** 
**Какие компании совершали перелеты на Boeing. Поля в результирующей таблице: name** 

```SQL
SELECT name
FROM Trip 
INNER JOIN Company 
ON company.id = trip.company
WHERE plane = 'Boeing' 
GROUP BY name;
```

---
#### ✅***Задание №7:*** 
**Вывести все названия самолётов, на которых можно улететь в Москву (Moscow). Поля в результирующей таблице: plane** 

```SQL
SELECT DISTINCT(plane)
FROM Trip
WHERE town_to = 'Moscow';
```

---
#### ✅***Задание №8:*** 
**В какие города можно улететь из Парижа (Paris) и сколько времени это займёт? Используйте конструкцию "as flight_time" для вывода необходимого времени. Это необходимо для корректной проверки.
Формат для вывода времени: HH:MM:SS Поля в результирующей таблице: town_to flight_time** 

```SQL
SELECT town_to,
TIMEDIFF(time_in, time_out) as flight_time
FROM Trip
WHERE town_from = 'Paris' ;
```

---
#### ✅***Задание №9:*** 
**Какие компании организуют перелеты из Владивостока (Vladivostok)? Поля в результирующей таблице: name** 

```SQL
SELECT name
FROM company
INNER JOIN trip
ON company.id = trip.company
WHERE town_from = 'Vladivostok';
```

---
#### ✅***Задание №10:*** 
**Вывести вылеты, совершенные с 10 ч. по 14 ч. 1 января 1900 г. Поля в результирующей таблице:** \* 

```SQL
SELECT *
FROM trip
WHERE time_out
BETWEEN '1900-01-01 10:00:00'
AND '1900-01-01 14:00:00';
```

---
