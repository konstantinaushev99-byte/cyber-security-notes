# Wireshark(ICMP)

---

# Установка
```bash
sudo apt update
sudo apt install wireshark -y
```
### Запуск
```bash
sudo wireshark
```

---

# Практика
1. Открыл Wireshark посмотрел интерфейс и увидел:
   - <img width="993" height="719" alt="image" src="https://github.com/user-attachments/assets/cbdd3a8a-60e4-4aa8-95d2-9bf93b233b4e" />
   - Вижу что есть eth0
   - Нажал на eth0 , и начался захват трафика
2. Ввел в консоль: ping 8.8.8.8 и оставил работать на 10 секунд
3. Найти ICMP:
   - В поле сверху ввел: ICMP
   - Увидел: <img width="263" height="379" alt="image" src="https://github.com/user-attachments/assets/b04ca7c8-37f0-4ca7-983a-dbcba2f7a274" />
4. Изучить пакеты:
   - Кликнул по пакету и раскрыл: Frame, Ethernet II, Internet Protocol Version 4, Internet Control Message Protocol
   - Открыл Ethernet II и посмотрел Source MAC(PCSSystemtec_67:68:3e (08:00:27:67:68:3e)) и Destination MAC(HuaweiDevice_e4:1f:42 (54:55:d5:e4:1f:42))
   - Открыл IPv4 и посмотрел Source IP(192.168.3.109) и Destination IP(8.8.8.8) и TTL: 64
   - Открыл ICMP и посмотрел Type 8 = Request, Type 0 = Reply
  
---

# Что запомнить:
- Что такое TTL? - TTL (Time To Live) — максимальное количество маршрутизаторов (hop), через которые может пройти пакет.
- ICMP Request (Type 8)
  ```bash
  Клиент → Сервер
  "Ты жив?"
  ```
- ICMP Reply (Type 0)
  ```bash
  Сервер → Клиент
  "Да, жив."
  ```

  

     

