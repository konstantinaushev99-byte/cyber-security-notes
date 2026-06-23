# DHCP

---

# Практика
1. На телефоне забыл Wi-Fi сеть и подключился заново
2. В Wireshark поставил фильтр: bootp
3. Увидел Discover и Request

---

# Что увидел в пакетах

**DHCP Discover:**
- src IP: 0.0.0.0 (телефон ещё не имеет IP)
- dst IP: 255.255.255.255 (broadcast — спрашивает всю сеть)

**DHCP Request:**
- src IP: 0.0.0.0
- dst IP: 255.255.255.255 (broadcast — подтверждает выбранный IP)

---

# Почему Offer и ACK не видны в Wireshark

Offer и ACK роутер отправляет unicast — напрямую на MAC телефона.
Мой компьютер этот трафик не получает, так как он адресован не ему.
Discover и Request видны, потому что они broadcast — их получают все устройства в сети.

---

а телефоне забыл Wi-Fi сеть и подключился заново
2. В Wireshark поставил фильтр: bootp
3. Увидел Discover и Request

---

# Что увидел в пакетах

**DHCP Discover:**
- src IP: 0.0.0.0 (телефон ещё не имеет IP)
- dst IP: 255.255.255.255 (broadcast — спрашивает всю сеть)

**DHCP Request:**
- src IP: 0.0.0.0
- dst IP: 255.255.255.255 (broadcast — подтверждает выбранный IP)

---

# Почему Offer и ACK не видны в Wireshark

Offer и ACK роутер отправляет unicast — напрямую на MAC телефона.
Мой компьютер этот трафик не получает, так как он адресован не ему.
Discover и Request видны, потому что они broadcast — их получают все устройства в сети.

Чтобы увидеть все 4 пакета нужен Port Mirroring на роутере.

---

# Итог DORA

| Шаг | Тип трафика | Видно в Wireshark? |
|-----|-------------|-------------------|
| Discover | Broadcast  |  Да |
| Offer    | Unicast    |  Нет (не адресован нам) |
| Request  | Broadcast  |  Да |
| ACK      | Unicast    |  Нет (не адресован нам) |
