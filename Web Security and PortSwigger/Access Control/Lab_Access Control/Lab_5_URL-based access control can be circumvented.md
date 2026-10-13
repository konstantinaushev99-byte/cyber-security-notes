# URL-based access control can be circumvented
## Цель: Обойти блокировку на основе контроля доступа и удалить пользователя Carlos

### Пути решения:
1. Так как у нас поддерживается заголовок X-Original-URL, и мы знаем что панель админа по адресу /admin
2. Находим любой запрос и отправляем , и добавляем заголовок X-Original-URL: /admin
3. У нас откроется панель админа,
4. Дальше чтобы удалить пользователя в строке запроса меняем путь ```GET /?username=carlos```, и к заголовку добавляем X-Original-Url: /admin/delete и нажимаем Send

Ссылка: https://portswigger.net/web-security/access-control/lab-url-based-access-control-can-be-circumvented
   
