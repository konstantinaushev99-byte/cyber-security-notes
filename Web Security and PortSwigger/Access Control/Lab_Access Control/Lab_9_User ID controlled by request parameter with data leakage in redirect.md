# User ID controlled by request parameter with data leakage in redirect
## Цель: В этой лабораторной работе обнаружена уязвимость в системе контроля доступа, при которой конфиденциальная информация утекает в теле ответа на перенаправление, найти API ключ и вести его в поле с ответом


### Пути решения:
1. Заходим в аккаунт использую свои учетные данные
2. Далее в URL запросе меняем id на carlos, нас выкинет на страницу входа
3. Но мы находим этот запрос в HTTP history и отправляем в Repeater, нажмаем на send
4. И ищем строку с API ключом и вставляем в поле для ответа

Ссылка: https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-data-leakage-in-redirect
