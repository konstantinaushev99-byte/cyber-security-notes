# Definition of Services

---

# Что делает -sV?
- При запуске команды: sudo nmap -sV 192.168.3.116
- Nmap может показать это:
    ```bash
    PORT    STATE SERVICE VERSION
    22/tcp  open  ssh     OpenSSH 9.6
    80/tcp  open  http    Apache httpd 2.4
- sV: показывает какая программа работает на порту
- sS: показывает какой порт открыт, закрыт или filtered

---

# Что такое Banner?
Banner — это информация о сервисе, которую он сам сообщает клиенту

Например:
1. SSH: SSH-2.0-OpenSSH_9.6
2. FTP: 220 FTP Server Ready
3. SMTP: 220 mail.example.com ESMTP
4. HTTP: Server: Apache/2.4.58

---

# Практика №1 - SSH
1. Запусти Wireshark с фильтром - tcp.port == 22
2. В терминале веду: sudo nmap -sV -p 22 192.168.3.116
3. В результате команды увидел это:
<img width="539" height="63" alt="image" src="https://github.com/user-attachments/assets/9a43a69c-5d67-4ddc-87ac-3887ec8a782f" />

Тут я увидел Программу: OpenSSH, Версию: 10.0, OC: Debian, и протокол: 2.0

5. Теперь вернемся в Wireshark, и заметим что сначало пошло подключение на проверку порта, после того как nmap проверил хост что он отвечает,он установил полное подключение, и ssh рассказал кто он и что он, после этого мы видим FIN,ACK - это значит что nmap перестал спрашиват ssh и хочет завершить подключение
<img width="1273" height="210" alt="image" src="https://github.com/user-attachments/assets/6256f8f0-04c3-4f01-afa7-85d03157ac34" />
6. И еще RST - это аварийное, мгновенное завершение соединения, а FIN - это нормальное завершение уже установленного соединения
