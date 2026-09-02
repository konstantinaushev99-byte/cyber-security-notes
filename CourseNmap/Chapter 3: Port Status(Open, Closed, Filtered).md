# Port Status

---

# Как это происходит?
1. Между клиентом и сервером стоит Firewall
2. Nmap отправляет: SYN
3. Firewall говорит: НЕТ , и просто выбрасывает пакет

Выглядит примерно так:
Nmap(SYN)--> Firewall(удалил пакет)--> Сервер
И тогда Nmap говорит - Я не могу определить, открыт порт или закрыт. Возможно, его фильтрует Firewall, и выводит - 80/tcp filtered

Очень важная таблица
| Ответ на SYN | Что думает Nmap |
|--------------|-----------------|
| SYN, ACK     | Open            |
| RST, ACK     | Closed          | 
| Нет ответа   | Filtered        | 

---

# Практика
1. Открыл 2 виртуальную машину Ubuntu и включил на ней firewall
2. Так же на Ubuntu настроил правило на блокировку 22 порта, чтобы увидеть filtered
3. Далее открыл Kali и запустил Wireshark с фильтром - tcp.port == 22
4. И в терминале ввел команду - sudo nmap -sS -p 22 192.168.3.116
5. В результате увидел - <img width="510" height="60" alt="image" src="https://github.com/user-attachments/assets/67eca77d-fb81-46be-85f8-c04faaa001e4" /> , что порт 22 - filtered
6. Также перешелл в wireshark, и увидел такую картину - <img width="1270" height="38" alt="image" src="https://github.com/user-attachments/assets/1ff2c9c2-4d90-4900-9c9e-9ab46ef29f81" />, пакеты SYN пошли , но без ответа.
7. Значит скорее всего стоит firewall.
