# SQL injection attack, querying the database type and version on Oracle
## Цель: Отоброзить строку с версией базы данных

### Пути решения:
1. Зайдем сразу в Intercept чтобы перехвать запрос
2. Включем его и откроем любую категорию товаров, далее добавим такую строку ```'+UNION+SELECT+BANNER,+NULL+FROM+v$version--```
3. Она нам и покажет версию базы данных

Ссылка: https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle
