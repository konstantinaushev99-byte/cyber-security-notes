# Wireshark(HTTP)
HTTP (HyperText Transfer Protocol) — протокол передачи веб-страниц между клиентом и сервером 
Работает на 80 порте 80/tcp

---
 
# HTTP Request - запрос от клиента к серверу
Основные методы: 
1. GET - получить данные
   ```bash
   GET / HTTP/1.1
   Host: example.com
2. POST - отправить данные
   ```bash
   POST /login
3. Host - домен сайта
   ```bash
   Host: google.com
4. User-Agent - информация о клиенте
   ```bash
   User-Agent: Chrome

---

# HTTP Response - ответ сервера
    ```bash
    HTTP/1.1 200 OK
Популярные статус-коды:
1. 200 OK - успех
2. 301 / 302 - перенаправление
3. 403 Forbidden - доступ запрещен
4. 404 Not Found - страница не найдена
5. 500 Internal Server Error - ошибка сервера

---

# Фильтры Wireshark
1. Показать HTTP: http 
2. Показать GET: http.request.method == "GET"
3. Показать POST: http.request.method == "POST"
4. Показать ответы: http.response

---

# Wireshark
1. Ввел в фильтр: HTTP
2. Открыл сайт neverssl.com так как он работает на HTTP
3. Открыл и нашел:
   - Метод запроса: GET
   - Host: neverssl.com
   - User-Agent: mozila/5.0
   - HTTP Status Code: 200 OK
   - Source IP: 192.168.3.109
   - Destination IP: 34.223.124.45
  
---

# Для собеседования
1. Что такое HTTP? - Протокол передачи веб-страниц между клиентом и сервером
2. На каком порту работает HTTP? - 80/TCP
3. Что такое GET? - Запрос на получение данных
4. Что такое POST? - Запрос на отправку данных
5. Что означает 404? - Страница не найдена
6. Что означает 200 OK? - Запрос выполнен успешно
7. Как отфильтровать HTTP в Wireshark? - http
