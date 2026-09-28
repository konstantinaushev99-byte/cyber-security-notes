# Username enumeration via different responses 
## Цель: узнать пароль и имя учетной записи с помощью brute-force attacks

## Пути решения lab:
1. Открыл Lab в Burp
2. Ввел в строку входа в аккаунт любые данные и нашел этот запрос(POST /login)  в HTTP history 
3. Далее выделил значение username, и отправил в Send to Intruder
4. Выбрал режим атаки Sniper и поставил Payload type: Simple list
5. В Payload configuration добавил все имена пользователей и начал выполнять атаку
6. После завершения перебора, отсортировал по длине и среди всех имен только одно будет выделятся(оно длинее всех)
7. Так же в правильном имени пользователя, во вкладке (ПКМ по запросу -> Response) будет Incorrect password, в остальных случаях будет Invalid username
8. После того как нашли нужное имя, закрывает атаку и возращаемся в Intruder, там нажал clear§
9. Дальше добавляем позицию полезной загрузки для password, и так же обновляем payload configuration и так же начинаем атаку
10. После завершения атаки смотрим не на длину, а на статус-код, среди всех одинаоквых только один будет отличатся в моем случае среди 200 , только один 302
11. Входим в аккаунт и lab пройдена

Ссылка на lab - https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-different-responses
