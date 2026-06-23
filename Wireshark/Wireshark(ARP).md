# ARP - Address Resolution Protocol

---

# Как выглядит ARP
Запрос:
```bash
Who has 192.168.3.94?
Tell 192.168.3.109

Перевод:
Кто владеет IP 192.168.3.94?
Сообщите 192.168.3.109
```

---

# Практика 
1. Ввел в Wireshark фильтр arp
2. Следом в терминале сделал - ping 192.168.3.112
3. После этого в Wireshark увидел это: <img width="1258" height="33" alt="image" src="https://github.com/user-attachments/assets/d0b54f94-40a0-4878-9b76-126385cbc6e0" />
4. Открыл ARP Request и нашёл:
 - Sender IP: 192.168.3.109 (мой компьютер)
 - Sender MAC: HuaweiDevice_e4:1f:42 (54:55:d5:e4:1f:42)
 - Target IP: 192.168.3.112 (кого ищем)
 - Target MAC: 00:00:00:00:00:00 (неизвестен)
5. Открыл ARP Reply и нашёл:
   - Sender IP: 192.168.3.112
   - Sender MAC: b2:72:53:95:64:ba (вот он!)
   - Target IP: 192.168.3.109
  
---

# Итог
- ARP отвечает на вопрос: "Я знаю IP. Какой у него MAC?"
- Сначала идёт ARP, только потом ICMP (ping)
- ARP Request — Broadcast (не знаем кому спрашивать, спрашиваем всех)
- ARP Reply — Unicast (уже знаем кому отвечать — Tell 192.168.3.109)
