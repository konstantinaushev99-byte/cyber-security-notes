# User ID controlled by request parameter
## Цель: получить API-ключ пользователя и отправить его в качестве решения

### Пути решения:
1. У нас есть имя жертвы и наши данные от учетной записи
2. Входим в учетную запись под своими данными и меняем свой id , на id жертвы
3. Было - "https://0a4800a7040f72eb8a14b80e00440035.web-security-academy.net/my-account?id=wiener"
4. Стало - "https://0a4800a7040f72eb8a14b80e00440035.web-security-academy.net/my-account?id=carlos"
5. И мы поподаем на страницу жертвы где находим API ключ
6. Сверху на странице находи кнопку - "Submit solution", туда вводим ключ

Ссылка: https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter
