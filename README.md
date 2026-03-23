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
WHERE name REGEXP 'man$';
```

---
#### ✅***Задание №5:*** 
**Вывести количество рейсов, совершенных на TU-134. Используйте конструкцию "as count" для агрегатной функции подсчета количества рейсов. Это необходимо для корректной проверки. Поля в результирующей таблице: count** 

```SQL
SELECT COUNT(id) as count
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
#### ✅***Задание №11:*** 
**Выведите пассажиров с самым длинным ФИО. Пробелы, дефисы и точки считаются частью имени. Поля в результирующей таблице: name** 

```SQL
SELECT name
FROM passenger
WHERE LENGTH(name) = (
SELECT MAX(LENGTH(name))
FROM passenger);
```

---
#### ✅***Задание №12:*** 
**Выведите идентификаторы всех рейсов и количество пассажиров на них. Обратите внимание, что на каких-то рейсах пассажиров может не быть. В этом случае выведите число "0". Используйте конструкцию "as count" для агрегатной функции подсчета количества пассажиров. Это необходимо для корректной проверки. Поля в результирующей таблице: id count**

```SQL
SELECT t.id,
COUNT(pt.Passenger) as count
FROM trip t
LEFT JOIN Pass_in_trip pt 
ON t.id = pt.trip
GROUP BY t.id;
```

---
#### ✅***Задание №13:*** 
**Вывести имена людей, у которых есть полный тёзка среди пассажиров. Поля в результирующей таблице: name** 

```SQL
SELECT name
FROM Passenger
GROUP BY name
HAVING COUNT(*) >= 2;
```

---
#### ✅***Задание №14:*** 
**В какие города летал Bruce Willis. Поля в результирующей таблице: town_to** 

```SQL
SELECT town_to
FROM passenger ps
JOIN Pass_in_trip pt
ON ps.id = pt.passenger
JOIN trip tr
ON tr.id = pt.trip
WHERE name = 'Bruce Willis'
GROUP BY town_to;
```

---
#### ✅***Задание №15:*** 
**Выведите идентификатор пассажира Стив Мартин (Steve Martin) и дату и время его прилёта в Лондон (London). Поля в результирующей таблице: id time_in** 

```SQL
SELECT ps.id as id,
t.time_in
FROM trip t 
JOIN Pass_in_trip pt
ON t.id = pt.trip
JOIN passenger ps
ON pt.passenger = ps.id
WHERE ps.name = 'Steve Martin'
AND t.town_to = 'London';
```

---
#### ✅***Задание №16:*** 
**Вывести отсортированный по количеству перелетов (по убыванию) и имени (по возрастанию) список пассажиров, совершивших хотя бы 1 полет. Используйте конструкцию "as count" для агрегатной функции подсчета количества перелетов. Это необходимо для корректной проверки. Поля в результирующей таблице: name count** 

```SQL
SELECT ps.name,
COUNT(*) as count
FROM Passenger ps 
JOIN Pass_in_trip pt
ON ps.id = pt.passenger
JOIN trip t
ON pt.trip = t.id
GROUP BY ps.name
HAVING COUNT(*) >= 1
ORDER BY COUNT DESC,
ps.name ASC;
```

---
#### ✅***Задание №17:*** 
**Определить, сколько потратил в 2005 году каждый из членов семьи. В результирующей выборке не выводите тех членов семьи, которые ничего не потратили. Используйте конструкцию "as costs" для отображения затраченной суммы членом семьи. Это необходимо для корректной проверки. Поля в результирующей таблице: member_name status costs** 

```SQL
SELECT fm.member_name,
fm.status,
SUM(p.unit_price * p.amount) AS costs
FROM FamilyMembers fm
JOIN Payments p
ON fm.member_id = p.family_member
WHERE EXTRACT(YEAR FROM p.date) = 2005
GROUP BY fm.member_name,
fm.status
HAVING SUM(p.unit_price * p.amount) > 0
ORDER BY costs DESC;
```

---
#### ✅***Задание №18:*** 
**Выведите имя самого старшего человека. Если таких несколько, то выведите их всех. Поля в результирующей таблице: member_name** 

```SQL
SELECT member_name
FROM FamilyMembers
WHERE birthday = (
SELECT MIN(birthday)
FROM FamilyMembers);
```

---
#### ✅***Задание №19:*** 
**Определить, кто из членов семьи покупал картошку (potato). Поля в результирующей таблице: status** 

```SQL
SELECT DISTINCT fm.status
FROM FamilyMembers fm
JOIN Payments p
ON fm.member_id = p.family_member
JOIN Goods g
ON p.good = g.good_id
WHERE g.good_name = 'potato';
```

---
#### ✅***Задание №20:*** 
**Сколько и кто из семьи потратил на развлечения (entertainment). Вывести статус в семье, имя, сумму. Используйте конструкцию "as costs" для отображения затраченной суммы членом семьи. Это необходимо для корректной проверки. Поля в результирующей таблице: status member_name costs** 

```SQL
SELECT fm.status,
fm.member_name,
SUM(ps.amount * ps.unit_price) AS costs
FROM FamilyMembers fm
JOIN Payments ps
ON fm.member_id = ps.family_member
JOIN Goods gs
ON ps.good = gs.good_id
JOIN GoodTypes gt
ON gs.type = gt.good_type_id
WHERE gt.good_type_name = 'entertainment'
GROUP BY fm.status,
fm.member_name
ORDER BY costs DESC;
```

---
#### ✅***Задание №21:*** 
**Определить товары, которые покупали более 1 раза. Поля в результирующей таблице: good_name** 

```SQL
SELECT good_name
FROM Goods g
JOIN Payments p
ON g.good_id = p.good
GROUP BY good_name
HAVING COUNT(p.good) > 1;
```

---
#### ✅***Задание №22:*** 
**Найти имена всех матерей (mother). Поля в результирующей таблице: member_name** 

```SQL
SELECT member_name
FROM FamilyMembers
WHERE status = 'mother';
```

---
#### ✅***Задание №23:*** 
**Найдите самый дорогой деликатес (delicacies) и выведите его цену. Поля в результирующей таблице: good_name unit_price** 

```SQL
SELECT good_name,
unit_price
FROM Payments
JOIN Goods
ON Payments.good = Goods.good_id
JOIN GoodTypes
ON Goods.type = GoodTypes.good_type_id
WHERE good_type_name = 'delicacies' LIMIT 1;
```

---
#### ✅***Задание №24:*** 
**Определить, кто и сколько потратил в июне 2005. Используйте конструкцию "as costs" для отображения затраченной суммы членом семьи. Это необходимо для корректной проверки. Поля в результирующей таблице: member_name
costs** 

```SQL
SELECT member_name,
SUM(amount * unit_price) as costs
FROM FamilyMembers
JOIN Payments
ON FamilyMembers.member_id=Payments.family_member
WHERE DATE_FORMAT(Payments.date, '%m.%Y') = '06.2005'
GROUP BY FamilyMembers.member_name;
```

---
#### ✅***Задание №25:*** 
**Определить, какие товары не покупались в 2005 году. Все доступные к покупке продукты находятся в таблице Goods. Поля в результирующей таблице: good_name** 

```SQL
SELECT good_name
FROM Goods
WHERE good_id NOT IN (
SELECT good
FROM Payments
WHERE YEAR(date) = '2005');
```

---
#### ✅***Задание №26:*** 
**Определить группы товаров, которые не приобретались в 2005 году. Поля в результирующей таблице: good_type_name** 

```SQL
SELECT gt.good_type_name
FROM GoodTypes as gt
LEFT JOIN Goods as g
ON gt.good_type_id = g.type
LEFT JOIN Payments as p
ON g.good_id = p.good AND YEAR(p.date) = 2005
GROUP BY gt.good_type_name
HAVING COUNT(p.payment_id) = 0;
```

---
#### ✅***Задание №27:*** 
**Узнайте, сколько было потрачено на каждую из групп товаров в 2005 году. Выведите название группы и потраченную на неё сумму. Если потраченная сумма равна нулю, т.е. товары из этой группы не покупались в 2005 году, то не выводите её. Используйте конструкцию "as costs" для отображения затраченной суммы на конкретную группу товаров. Это необходимо для корректной проверки. Поля в результирующей таблице: good_type_name costs** 

```SQL
SELECT good_type_name,
SUM(amount*unit_price) AS costs
FROM GoodTypes
JOIN Goods
ON good_type_id = type
JOIN Payments
ON good = good_id AND YEAR(date) = 2005
GROUP BY good_type_name;
```

---
#### ✅***Задание №28:*** 
**Сколько рейсов совершили авиакомпании из Ростова (Rostov) в Москву (Moscow)? Используйте конструкцию "as count" для агрегатной функции подсчета количества рейсов. Это необходимо для корректной проверки. Поля в результирующей таблице: count** 

```SQL
SELECT COUNT(town_from) AS count
FROM Trip 
WHERE town_from='Rostov' AND town_to='Moscow';
```

---
#### ✅***Задание №29:*** 
**Выведите имена пассажиров, улетевших в Москву (Moscow) на самолете TU-134. В ответе не должно быть дубликатов. Поля в результирующей таблице: name** 

```SQL
SELECT DISTINCT Passenger.name
FROM Passenger
JOIN Pass_in_trip
ON Passenger.id = Pass_in_trip.passenger
JOIN Trip
ON Pass_in_trip.trip = Trip.id
WHERE Trip.plane = 'TU-134' AND Trip.town_to = 'Moscow';
```

---
#### ✅***Задание №30:*** 
**Вывести количество занятых мест по каждому рейсу из таблицы Pass_in_trip, отсортировав результат по убыванию количества занятых мест. Используйте конструкцию "as count" для агрегатной функции подсчета числа пассажиров на рейсе. Это необходимо для корректной проверки. Поля в результирующей таблице: trip count** 

```SQL
SELECT trip,
COUNT(passenger) AS count
FROM Pass_in_trip
GROUP BY trip
ORDER BY count DESC;
```

---
#### ✅***Задание №31:*** 
**Вывести всех членов семьи с фамилией Quincey. Поля в результирующей таблице:** \* 

```SQL
SELECT *
FROM FamilyMembers
WHERE  member_name LIKE '%_Quincey';
```

---
#### ✅***Задание №32:*** 
**Вывести средний возраст людей (в годах), хранящихся в базе данных. Результат округлите до целого в меньшую сторону. Используйте конструкцию "as age" для агрегатной функции подсчета среднего возраста. Это необходимо для корректной проверки. Поля в результирующей таблице: age**

```SQL
SELECT FLOOR(AVG(TIMESTAMPDIFF(YEAR, birthday, NOW()))) AS age
FROM FamilyMembers;
```

---
#### ✅***Задание №33:*** 
**Найдите среднюю цену икры на основе данных, хранящихся в таблице Payments. В базе данных хранятся данные о покупках красной (red caviar) и черной икры (black caviar). В ответе должна быть одна строка со средней ценой всей купленной когда-либо икры. Используйте конструкцию "as cost" для агрегатной функции подсчета средней цены икры. Это необходимо для корректной проверки. Поля в результирующей таблице: cost**

```SQL
SELECT AVG(unit_price) as cost
FROM Payments
JOIN Goods
ON Payments.good=Goods.good_id
WHERE Goods.good_name='red caviar' OR  Goods.good_name='black caviar';
```

---
#### ✅***Задание №34:*** 
**Сколько всего 10-ых классов. Используйте конструкцию "as count" для агрегатной функции подсчета количества классов. Это необходимо для корректной проверки. Поля в результирующей таблице: count**

```SQL
SELECT distinct count(name) as count
FROM Class
WHERE name LIKE '10_%';
```

---
#### ✅***Задание №35:*** 
**Сколько различных кабинетов школы использовались 2 сентября 2019 года для проведения занятий? Используйте конструкцию "as count" для агрегатной функции подсчета количества различных кабинетов. Это необходимо для корректной проверки. Поля в результирующей таблице: count**

```SQL
SELECT COUNT(DISTINCT classroom) AS count
FROM Schedule
WHERE date LIKE "2019-09-02%";
```

---
#### ✅***Задание №36:*** 
**Выведите информацию об обучающихся, живущих на улице Пушкина (ul. Pushkina)? Поля в результирующей таблице:** \*

```SQL
SELECT *
FROM  Student
WHERE address LIKE '%_Pushkina%';
```

---
#### ✅***Задание №37:*** 
**Сколько лет самому молодому обучающемуся? Используйте конструкцию "as year" для указания возраста учащегося. Это необходимо для корректной проверки. Поля в результирующей таблице: year**

```SQL
SELECT TIMESTAMPDIFF(YEAR,MAX(birthday),NOW()) as year
FROM Student;
```

---
#### ✅***Задание №38:*** 
**Сколько учениц с именем Анна (Anna) учится в школе? Используйте конструкцию "as count" для агрегатной функции подсчета количества учащихся. Это необходимо для корректной проверки. Поля в результирующей таблице:
count**

```SQL
SELECT COUNT(first_name) as count
FROM Student
WHERE first_name='Anna';
```

---
#### ✅***Задание №39:*** 
**Сколько обучающихся в 10 B классе? Используйте конструкцию "as count" для агрегатной функции подсчета количества учащихся. Это необходимо для корректной проверки. Поля в результирующей таблице: count**

```SQL
SELECT COUNT(student) AS count
FROM Student_in_class
JOIN Class
ON Student_in_class.class = Class.id
WHERE Class.name = '10 B';
```

---
#### ✅***Задание №40:*** 
**Выведите название предметов, которые преподает Ромашкин П.П. (Romashkin P.P.). Обратите внимание, что в базе данных есть несколько учителей с такой фамилией. Используйте конструкцию "as subjects" для указания учебных предметов. Это необходимо для корректной проверки. Поля в результирующей таблице: subjects**

```SQL
SELECT name as subjects
FROM Teacher
JOIN Schedule
ON Schedule.teacher = Teacher.id
JOIN Subject
ON Subject.id = Schedule.subject
WHERE first_name LIKE 'P%'
AND middle_name LIKE 'P%'
AND last_name LIKE 'Romashkin';
```

---
#### ✅***Задание №41:*** 
**Выясните, во сколько по расписанию начинается четвёртое занятие. Поля в результирующей таблице: start_pair**

```SQL
SELECT start_pair
FROM Timepair
WHERE start_pair LIMIT 1 OFFSET 3;
```

---
#### ✅***Задание №42:*** 
**Сколько времени обучающийся будет находиться в школе, учась со 2-го по 4-ый уч. предмет? Используйте конструкцию "as time" для указания разницы во времени. Это необходимо для корректной проверки. Результат должен быть в формате HH:MM:SS Поля в результирующей таблице: time**

```SQL
SELECT DISTINCT TIMEDIFF(
(SELECT end_pair
FROM Timepair
WHERE id = '4'),
(SELECT start_pair
FROM Timepair
WHERE id = '2')
) AS time
FROM Timepair;
```

---
#### ✅***Задание №43:*** 
**Выведите фамилии преподавателей, которые ведут физическую культуру (Physical Culture). Отсортируйте преподавателей по фамилии в алфавитном порядке. Поля в результирующей таблице: last_name**

```SQL
SELECT last_name
FROM Teacher
JOIN Schedule
ON Teacher.id=Schedule.teacher
JOIN Subject
ON Schedule.subject=Subject.id
WHERE Subject.name='Physical Culture'
GROUP BY last_name
ORDER BY last_name ASC;
```

---
#### ✅***Задание №44:*** 
**Найдите максимальный возраст (количество лет) среди обучающихся 10 классов на сегодняшний день. Для получения текущих даты и времени используйте функцию NOW(). Используйте конструкцию "as max_year" для указания максимального возраста в годах. Это необходимо для корректной проверки. Поля в результирующей таблице: max_year**

```SQL
SELECT MAX(TIMESTAMPDIFF(YEAR,birthday,CURRENT_DATE)) AS max_year
FROM Student
JOIN Student_in_class
ON Student.id = Student_in_class.student
JOIN Class
ON Student_in_class.class = Class.id
WHERE Class.name LIKE '10%';
```

---
#### ✅***Задание №45:*** 
**Какие кабинеты чаще всего использовались для проведения занятий? Выведите те, которые использовались максимальное количество раз. Поля в результирующей таблице: classroom**

```SQL
SELECT classroom
FROM Schedule
GROUP BY classroom
HAVING COUNT(classroom) = (
SELECT COUNT(classroom)
FROM Schedule
GROUP BY classroom
ORDER BY COUNT(classroom) DESC LIMIT 1);
```

---
#### ✅***Задание №46:*** 
**В каких классах ведёт занятия преподаватель "Krauze"? Поля в результирующей таблице: name**

```SQL
SELECT name
FROM Class
JOIN Schedule
ON Class.id=Schedule.class
JOIN Teacher
ON Schedule.teacher=Teacher.id
WHERE last_name='Krauze'
GROUP BY name;
```

---
#### ✅***Задание №47:*** 
**Сколько занятий провел Krauze 30 августа 2019 г.? Используйте конструкцию "as count" для агрегатной функции подсчета числа занятий. Это необходимо для корректной проверки. Поля в результирующей таблице: count**

```SQL
SELECT COUNT(teacher) AS count
FROM Schedule
WHERE teacher = (
SELECT id
FROM Teacher
WHERE last_name = 'Krauze')
AND DATE(date) = '2019-08-30';
```

---
#### ✅***Задание №48:*** 
**Выведите заполненность классов в порядке убывания. Используйте конструкцию "as count" для агрегатной функции подсчета числа учащихся в классах. Это необходимо для корректной проверки. Поля в результирующей таблице:
name count**

```SQL
SELECT name,
COUNT(Student_in_class.student) AS count
FROM Class
JOIN Student_in_class
ON Student_in_class.class = Class.id
GROUP BY Class.name
ORDER BY count DESC;
```

---
#### ✅***Задание №49:*** 
**Какой процент обучающихся учится в "10 A" классе? Выведите ответ в диапазоне от 0 до 100 с округлением до четырёх знаков после запятой, например, 96.0201. Используйте конструкцию "as percent" для представления результата вычисления. Это необходимо для корректной проверки. Поля в результирующей таблице: percent**

```SQL
SELECT COUNT(student) * 100 / (
SELECT COUNT(student)
FROM Student_in_class) AS percent
FROM Student_in_class
JOIN Class
ON Student_in_class.class = Class.id
WHERE name='10 A';

-- или так

SELECT ROUND((
SELECT COUNT(*)
FROM Student_in_class
WHERE class IN (
SELECT id
FROM Class
WHERE name = '10 A'
)) * 100.0 / COUNT(*), 4) AS percent
FROM Student_in_class;
```

---
#### ✅***Задание №50:*** 
**Какой процент обучающихся родился в 2000 году? Результат округлить до целого в меньшую сторону. Используйте конструкцию "as percent" для указания процента. Это необходимо для корректной проверки. Поля в результирующей таблице: percent**

```SQL
SELECT FLOOR(COUNT(birthday) * 100.0 / (
SELECT COUNT(*)
FROM Student)) AS percent
FROM Student
WHERE birthday LIKE '2000%';
```

---
#### ✅***Задание №51:*** 
**Добавьте товар с именем "Cheese" и типом "food" в список товаров (Goods). В качестве первичного ключа (good_id) укажите количество записей в таблице + 1.**

```SQL
INSERT INTO Goods (good_id, good_name, type)
SELECT (
SELECT COUNT(*) + 1
FROM Goods),
'Cheese',(
SELECT good_type_id
FROM GoodTypes
WHERE good_type_name = 'food');
```

---
#### ✅***Задание №52:*** 
**Добавьте в список типов товаров (GoodTypes) новый тип "auto". В качестве первичного ключа (good_type_id) укажите количество записей в таблице + 1.**

```SQL
INSERT INTO GoodTypes (good_type_id, good_type_name)
SELECT (
SELECT COUNT(*) + 1
FROM GoodTypes),
'auto';
```

---
#### ✅***Задание №53:*** 
**Измените имя "Andie Quincey" на новое "Andie Anthony".**

```SQL
UPDATE FamilyMembers 
SET member_name = 'Andie Anthony'
WHERE member_name = 'Andie Quincey';
```

---
#### ✅***Задание №54:*** 
**Удалить всех членов семьи с фамилией "Quincey".**

```SQL
DELETE
FROM FamilyMembers
WHERE member_name LIKE '%Quincey';
```

---
