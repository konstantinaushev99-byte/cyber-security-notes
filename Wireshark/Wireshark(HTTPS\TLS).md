# Wireshark HTTPS/TLS
Что это - HTTP внутри защищённого TLS-туннеля
TLS (Transport Layer Security) шифрует данные между клиентом и сервером

Поэтому вместо: GET /login 
В wireshark мы увидим: Application Data (зашифровано)

---

# Последовательность соединения:
Для HTTPS:
DNS
↓
TCP Handshake
↓
TLS Handshake
↓
Encrypted Data

---

# TLS Handshake
Шаг 1: Client Hello
Шаг 2: Server Hello
Шаг 3: Сервер отправляет сертификат 
  - Например: CN=google.com
  - Браузер проверяет:
    - сертификат действителен?
    - кто его выдал?
    - срок не истёк?
Шаг 4: Клиент и сервер договариваются о ключе шифрования
  - После этого начинается: Encrypted Application Data

---

# Что можно увидеть в Wireshark
Даже без расшифровки можно увидеть:
1. IP клиента: 192.168.3.109
2. IP сервера: 142.250.x.x
3. Client Hello: TLSv1.3 Client Hello
4. Server Hello: Server Hello
5. Сертификат: Certificate
6. SNI:
   ```bash
   Client Hello
   ↓
   Extensions
   ↓
   Server Name Indication

   Там увидишь: google.com или github.com

---

# Что уже НЕ видно
После TLS Handshake не видно:
1. URL
2. Логины
3. Пароли
4. Содержимое страниц
Вместо этого:
```bash
Application Data
Application Data
Application Data
```

---

# Практика
1. Открыть: google.com
2. В Wireshark фильтр: tls
3. Увидел это: <img width="1261" height="92" alt="image" src="https://github.com/user-attachments/assets/0b2770c3-e451-4ec1-89be-ea308dd71890" />
4. Открыл Client Hello и перешел по данному путю и нашел домен mozilla.net:
   <img width="329" height="130" alt="image" src="https://github.com/user-attachments/assets/84a59d4f-b3ba-4e2d-9bbf-e1ef22afd937" />
5. Также открыл Cipher Suites и нашел поддерживаные шифрования: <img width="898" height="483" alt="image" src="https://github.com/user-attachments/assets/be0624bc-a6bd-42dc-b43b-d62f9686e623" />
6. Зайдя в Server Hello увидел какой тип шифрования будет использовать сервер: <img width="696" height="118" alt="image" src="https://github.com/user-attachments/assets/8327efbc-02b3-4306-8ebf-d0292eb56aeb" />

---

# Для собеседования
1. Что такое HTTPS? - HTTP поверх TLS
2. Для чего нужен TLS? - Для шифрования трафика
3. Что такое Client Hello? - Первое сообщение TLS от клиента
4. Что такое Server Hello? - Ответ сервера с выбранными параметрами шифрования
5. Что такое сертификат? - Подтверждение подлинности сервера
6. Что такое SNI? - Поле в Client Hello, содержащее имя сайта
