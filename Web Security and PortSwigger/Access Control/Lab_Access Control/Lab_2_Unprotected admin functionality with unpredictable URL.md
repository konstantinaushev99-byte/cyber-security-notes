# Unprotected admin functionality with unpredictable URL 
## Цель: Панель администратора находится в непредсказуемом месте, местоположение указано где-то в приложении, найти ее и удалить пользователя Carlos

### Пути решения:
1. Сделаем POST запрс и отправил его в Repeater нажмем Send
2. И в заголовке <script> надем админ панел - ```/admin-ghjfvt```
3. Так же это можно сделать неиспользуя Repeater, а просто просмотреть код страницы
4. Далее входит и удаляем пользователя Carlos

Ссылка: https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality-with-unpredictable-url
