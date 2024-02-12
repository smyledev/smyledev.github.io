# Запросы SQL

## Задание 1

  Дан csv файл с историей заказов (client_id - торговая точка, purchase_date - дата закупки). Надо положить этот csv файл в SQL базу данных и написать запросы, чтобы посчитать для каждого месяца количество:
  1. Новых торговых точек (впервые за всю историю сделали заказ в этом месяце).
  2. Торговых точек, которые сделали заказ в прошлом месяце и в этом.
  3. Торговых точек, которые когда-то что-то заказывали (только не в прошлом месяце) и вернувшиеся в этом месяце.
  4. Торговых точек отвалившихся (ничего не купили) в этом месяце. 
  
  Представление данных:
  client_id,purchase_date 
  150,6/18/2015
  160,6/23/2015
  ...

## Решение задания 1

### Запрос 1

```sql
CREATE TEMPORARY TABLE Monthes 
SELECT DISTINCT SUBSTRING(historyoforders.purchase_date, 1, 1) AS month
FROM historyoforders;

CREATE TEMPORARY TABLE MonthesWithHistoryOfOrders
SELECT Monthes.month, historyoforders.purchase_date, historyoforders.client_id 
FROM Monthes
LEFT JOIN historyoforders 
ON month = SUBSTRING(historyoforders.purchase_date, 1, 1);

CREATE TEMPORARY TABLE CountOfPrevMonthes
SELECT month, purchase_date, client_id, 
(SELECT COUNT(сurrentMonth.client_id)
 FROM MonthesWithHistoryOfOrders AS сurrentMonth
 WHERE сurrentMonth.client_id = previousMonthes.client_id
 AND сurrentMonth.month < previousMonthes.month) AS countOfMonthes
FROM MonthesWithHistoryOfOrders previousMonthes;

CREATE TEMPORARY TABLE NewPoints
SELECT month, purchase_date, client_id, countOfMonthes  FROM CountOfPrevMonthes
WHERE countOfMonthes = 0;

SELECT month, COUNT(client_id) FROM NewPoints
GROUP BY month 
```

### Запрос 2

```sql
CREATE TEMPORARY TABLE Monthes 
SELECT DISTINCT SUBSTRING(historyoforders.purchase_date, 1, 1) AS month
FROM historyoforders;

CREATE TEMPORARY TABLE MonthesWithJoin
SELECT Monthes.month, historyoforders.purchase_date, historyoforders.client_id 
FROM Monthes
LEFT JOIN historyoforders 
ON month = SUBSTRING(historyoforders.purchase_date, 1, 1);

CREATE TEMPORARY TABLE CountOfPrevMonth
SELECT month, purchase_date, client_id, 
(SELECT COUNT(сurrentMonth.client_id)
 FROM MonthesWithJoin AS сurrentMonth
 WHERE сurrentMonth.client_id = previousMonthes.client_id
 AND сurrentMonth.month = previousMonthes.month - 1) AS countOfMonthes
FROM MonthesWithJoin previousMonthes;

CREATE TEMPORARY TABLE CurrentAndPrevPoints
SELECT month, purchase_date, client_id, countOfMonthes  FROM CountOfPrevMonth
WHERE countOfMonthes = 1;

SELECT month, COUNT(client_id) FROM CurrentAndPrevPoints
GROUP BY month 
```

### Запрос 3

```sql
CREATE TEMPORARY TABLE Monthes 
SELECT DISTINCT SUBSTRING(historyoforders.purchase_date, 1, 1) AS month
FROM historyoforders;

CREATE TEMPORARY TABLE MonthesWithJoin
SELECT Monthes.month, historyoforders.purchase_date, historyoforders.client_id
FROM Monthes
LEFT JOIN historyoforders 
ON month = SUBSTRING(historyoforders.purchase_date, 1, 1);

CREATE TEMPORARY TABLE CountOfNotPrevMonthes
SELECT month, purchase_date, client_id, 
(SELECT COUNT(сurrentMonth.client_id)
 FROM MonthesWithJoin AS сurrentMonth
 WHERE сurrentMonth.client_id = notPreviousMonthes.client_id
 AND сurrentMonth.month <> notPreviousMonthes.month - 1) AS countOfMonthes
FROM MonthesWithJoin notPreviousMonthes;

CREATE TEMPORARY TABLE CurrentAndNotPrevPoints
SELECT month, purchase_date, client_id, countOfMonthes  FROM CountOfNotPrevMonthes
WHERE countOfMonthes = 1;

SELECT month, COUNT(client_id) FROM CurrentAndNotPrevPoints
GROUP BY month 
```


### Запрос 4

```sql
CREATE TEMPORARY TABLE Monthes 
SELECT DISTINCT SUBSTRING(historyoforders.purchase_date, 1, 1) AS month 
FROM historyoforders;

CREATE TEMPORARY TABLE MinMonth 
SELECT MIN(month) AS min_month 
FROM Monthes;

CREATE TEMPORARY TABLE CountOfMonthes
SELECT Monthes.month, historyoforders.purchase_date, historyoforders.client_id 
FROM Monthes
LEFT JOIN historyoforders 
ON month = SUBSTRING(historyoforders.purchase_date, 1, 1)
ORDER BY month, client_id;

CREATE TEMPORARY TABLE MatchingPointsForPrevMonthes
SELECT a.month, a.purchase_date as purchase_date1, a.client_id as client_id1, 
b.purchase_date as purchase_date2, b.client_id as client_id2
FROM CountOfMonthes a 
LEFT JOIN CountOfMonthes b
ON b.month < a.month AND a.month AND a.client_id = b.client_id 
WHERE b.client_id IS NULL
ORDER BY a.month, a.client_id;

SELECT MatchingPointsForPrevMonthes.month as month, COUNT(client_id1)
FROM MatchingPointsForPrevMonthes, MinMonth
WHERE month <> MinMonth.min_month
GROUP BY month 
```

[Назад](./)
