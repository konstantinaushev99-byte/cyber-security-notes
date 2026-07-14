# Source Port
Что это - --source-port позволяет Nmap изменить исходный порт отправляемого пакета. Это может помочь пройти через старые firewall, если они доверяют пакетам с определенных исходных портов (например, 53 или 20)

---

# Практика
1. Открою wireshark и введу фильтр:
   ```bash
   ip.addr == 192.168.3.116
2. Открою kali и выполню команду:
   ```bash
   sudo nmap --source-port 53 -sS -p 22 192.168.3.116
3. Вернусь в wireshark и увижу что порт с которого отправляли пакет изсенился:<img width="348" height="38" alt="image" src="https://github.com/user-attachments/assets/6474c9df-f2c9-4239-9a94-b8d96fd46d11" />
4. А без --source-port, будет вот так: <img width="398" height="40" alt="image" src="https://github.com/user-attachments/assets/13361b4a-5b89-4ed6-aa1c-b0414a4bffae" />

Получается очень наглядное сравнение
| Без --source-port | С --source-port 53 |
| Source Port = 63576 | Source Port = 53 |
| Destination Port = 22 | Destination Port = 22 |
