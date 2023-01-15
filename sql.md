# SQL. Основные конструкции языка и решения задач.

## Задания с решениями

1. В БД есть таблица Users (Name, Country). Нужно написать SQL запрос, который вернёт список стран в алфавитном порядке, в которых количество пользователей больше 1000.

```sql
SELECT `Country`, COUNT(`Country`) as `cnt`
FROM `users`
GROUP BY `Country`
HAVING `cnt` > 1000
ORDER BY `Country`
```

[Назад](./)
