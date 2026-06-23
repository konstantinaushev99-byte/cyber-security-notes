# DHCP

---

1. DHCP автоматически выдает:
- IP адрес
- Маску сети
- Шлюз
- DNS сервер

---

# Практика Wireshark
1. Фильтр: 
- dhcp или bootp
2. Как поймать DHCP
- сначало это sudo dhclient -r eth0 ( отпустить IP )
- затем это sudo dhclient eth0 ( запросить новый )


