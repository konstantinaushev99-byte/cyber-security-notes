# data-lenght
### Что делает - Добавляет случайные байты (payload) в отправляемый пакет 
### Зачем - Чтобы изменить размер и сигнатуру пакета, сделав его менее похожим на типичный пакет Nmap для старых IDS/IPS и firewall
### Почему сегодня почти не работает Потому что современные IDS/IPS анализируют не только размер пакета, но и поведение: частоту, TCP-флаги, последовательность пакетов, количество соединений и другие признаки

---

# Практика
1. В Wireshark введу фильтр - ip.addr == 192.168.3.116
2. В kali выполню:
   ```bash
   sudo nmap -sS -p 22 192.168.3.116
3. И после еще одну:
   ```bash
   sudo nmap -sS --data-length 50 -p 22 192.168.3.116
4. Вернувшись в wireshark можно увидеть что добавились случайные данные
5. было - <img width="419" height="106" alt="image" src="https://github.com/user-attachments/assets/ac809ff5-9da6-4205-8cf2-2f1667231f36" /> TCP Segment Len: 0
6. стало - <img width="194" height="103" alt="image" src="https://github.com/user-attachments/assets/c78c8feb-606a-496f-b362-45ec4389f832" /> TCP Segment Len: 50
