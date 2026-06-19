# TCP Handshake
Что это - это процесс установления надёжного соединения между клиентом и сервером по протоколу Transmission Control Protocol (TCP) перед началом передачи данных

---

Состоит из 3 пакетов:
```bash 
Клиент                 Сервер
SYN ----------------->
     <-------------- SYN, ACK
ACK ----------------->
Соединение установлено
```

---

# Действия: 
Шаг 1: SYN
- Клиент говорит: «Привет, хочу установить соединение»
- Пакет: SYN = 1 ACK = 0
Шаг 2: SYN-ACK
- Сервер отвечает: «Привет, запрос получил, готов общаться»
- Пакет: SYN = 1 ACK = 1
Шаг 3: ACK
- Клиент подтверждает: «Отлично, начинаем работу»
- Пакет: SYN = 0 ACK = 1

После этого начинается передача данных: HTTP, HTTPS, SSH, FTP, RDP, SMTP

---

# Как найти в Wireshark:
1. Фильтр: tcp или tcp.flags.syn == 1 или для полной цепочки tcp.stream == X(номер потока(индекс))
2. Что искать:
   - Первый пакет: [SYN]
   - Второй: [SYN, ACK]
   - Третий: [ACK]
3. Открываем любой браузер и смотри TCP Handshake в Wireshark
   - Пишем в фильтре: tcp.flags.syn == 1 и увидим что типо токого:
     <img width="1257" height="34" alt="image" src="https://github.com/user-attachments/assets/f2d10767-b446-4afd-bd5e-8550c6df7591" />
   - Далее переходим к ниму и ищем Transmission Control Protocol, там ищем Strem index: 10 (у всех он будет разный):
     <img width="620" height="367" alt="image" src="https://github.com/user-attachments/assets/7ca0b550-a41f-4513-a896-4886c70592a7" /> 
   - После этого пишем фильтр tcp.stream == X(номер потока(индекс)):
     <img width="1254" height="106" alt="image" src="https://github.com/user-attachments/assets/e54fa1c5-77e7-47a4-a59b-5c19b12f0877" />
   - И после этого увидим полную цепочку SYN -> SYN,ARC -> ARC
4. Видим также что:
   - IP клиента - 192.168.3.109
   - IP сервера - 199.232.173.91
   - порт клиента - 37560
   - порт сервера - 443

   ---

# Итоги:
- SYN → запрос на установку соединения
- ACK → подтверждение получения пакета
- TCP Handshake = SYN → SYN/ACK → ACK



