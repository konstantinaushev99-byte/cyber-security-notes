# Сетевые команды Linux

---

1. ip a - Показывает все сетевые интерфейсы и их IP-адреса (замена устаревшей ifconfig)
   ```bash
   ip a
   ip addr          # то же самое

   $ ip a
   1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
    inet 127.0.0.1/8 scope host lo
   2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP
    inet 192.168.1.100/24 brd 192.168.1.255 scope global dynamic eth0

  Что означают ключевые элементы:
  | Элемент |	Значение |
  |---------|----------|
  | lo      |	Loopback (локальный интерфейс, адрес 127.0.0.1) |
  | eth0, enp0s3, wlan0 |	Физические или виртуальные сетевые интерфейсы |
  | state UP | Интерфейс активен |
  | state DOWN | Интерфейс отключён |
  | inet | IPv4-адрес |
  | inet6 |	IPv6-адрес |
  | /24	| Маска подсети (255.255.255.0) |
  | dynamic |	Адрес получен через DHCP |
  | brd |	Broadcast-адрес |

---

2. ip route - Показывает маршруты, по которым система отправляет трафик
    ``` bash
   ip route
   ip r               # сокращённый вариант

   $ ip route
   default via 192.168.1.1 dev eth0 proto dhcp metric 100
   192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.100 metric 100

  Что означают ключевые элементы:
  | Элемент | Значение |
  |---------|----------|
  | default | Маршрут по умолчанию (куда уходит трафик в интернет) |
  | via 192.168.1.1 | Шлюз (роутер), через который идёт трафик |
  | dev eth0 | Интерфейс, через который идёт трафик |
  | 192.168.1.0/24 | Локальная сеть |
  | src 192.168.1.100 | Исходный IP-адрес для соединений | 
  | proto dhcp | Маршрут получен через DHCP |

---

3. ping - Отправляет ICMP-запросы (echo request) и ждёт ответа. Проверяет, доступен ли хост
   ```bash
   ping <IP_или_домен>

   ping google.com
   ping 8.8.8.8

   $ ping -c 4 google.com
   PING google.com (142.250.185.46) 56(84) bytes of data.
   64 bytes from 142.250.185.46: icmp_seq=1 ttl=117 time=15.2 ms
   64 bytes from 142.250.185.46: icmp_seq=2 ttl=117 time=14.8 ms
   64 bytes from 142.250.185.46: icmp_seq=3 ttl=117 time=15.1 ms
   64 bytes from 142.250.185.46: icmp_seq=4 ttl=117 time=14.9 ms

   --- google.com ping statistics ---
   4 packets transmitted, 4 received, 0% packet loss, time 3003ms
   rtt min/avg/max/mdev = 14.8/15.0/15.2/0.2 ms

  Что означают поля:
  | Поле | Значение |
  |------|----------|
  | icmp_seq | Номер пакета |
  | ttl | Time To Live (сколько прыжков осталось) |
  | time | Время ответа в миллисекундах | 
  | 0% packet loss | Потеря пакетов (0% — идеально) | 

  Полезные опции:
  | Опция | Что делает | Пример |
  |-------|------------|--------|
  | -c N  | Отправить N пакетов (иначе ping бесконечен) | ping -c 4 google.com |
  | -i N  | Интервал между пакетами (секунды) | ping -i 2 google.com |
  | -s N  | Размер пакета в байтах | ping -s 1024 google.com | 

---

4. traceroute - Показывает все промежуточные узлы (роутеры), через которые проходят пакеты до цели
   ```bash
   traceroute <IP_или_домен>

   $ traceroute google.com
   traceroute to google.com (142.250.185.46), 30 hops max, 60 byte packets
   1  _gateway (192.168.1.1)  1.234 ms  1.156 ms  1.089 ms
   2  10.0.0.1 (10.0.0.1)  5.432 ms  5.211 ms  5.098 ms
   3  * * *
   4  72.14.198.0 (72.14.198.0)  15.234 ms  15.098 ms  14.987 ms
   5  google.com (142.250.185.46)  16.001 ms  15.876 ms  15.654 ms

  Что означают элементы:
  | Элемент | Значение |
  |---------|----------|
  | hop 1, hop 2 | Каждый прыжок от одного роутера к другому |
  | * * * | Узел не ответил (может блокировать ICMP) |
  | ms | 	Время ответа на каждом прыжк |

  Полезные опции: 
  | Опция | Что делает | Пример |
  |-------|------------|--------|
  | -n    | Не преобразовывать IP в имена (быстрее) | traceroute -n google.com |
  | -m N  | Максимальное количество прыжков | traceroute -m 15 google.com |

---

5. nslookup - Запрашивает DNS-сервер, чтобы узнать IP-адрес по имени домена (или наоборот)
   ```bash
   nslookup google.com
   nslookup 8.8.8.8

   $ nslookup google.com
   Server:     192.168.3.1
   Address:    192.168.3.1#53

   Non-authoritative answer:
   Name:   google.com
   Address: 142.250.185.46

  Полезные опции:
  | Команда | Что делает |
  |---------|------------|
  | nslookup -type=MX google.com | Запросить MX-записи (почтовые серверы) |
  | nslookup -type=TXT google.com | Запросить TXT-записи | 
  | nslookup -debug google.com | Режим отладки (подробный вывод) | 

---

6. dig - Более мощная и гибкая версия nslookup. Показывает больше деталей
   ```bash
   dig google.com
   dig -x 8.8.8.8          # обратный запрос (IP → имя)

   $ dig google.com +short
   142.250.185.46

   $ dig google.com
   ;; QUESTION SECTION:
   ;google.com.   IN  A

   ;; ANSWER SECTION:
   google.com. 299  IN  A  142.250.185.46

   ;; SERVER: 192.168.3.1#53(192.168.3.1)

  Полезные опции:
  | Команда | Что делает |
  |---------|------------|
  | dig google.com +short | Только IP-адрес (кратко) |
  | dig google.com A | Запросить A-запись (IPv4) |
  | dig google.com AAAA | Запросить AAAA-запись (IPv6) |
  | dig google.com MX | Запросить MX-записи |
  | dig google.com NS | Запросить NS-записи (DNS-серверы домена) |
  | dig +trace google.com | Показать весь путь DNS-запроса (от корневых серверов) | 

---

7. ss - Показывает открытые сетевые соединения, порты, сокеты. Современная замена netstat (быстрее, даёт больше информации)
   ```bash
   ss -tulpn

   $ sudo ss -tulpn
   Netid  State   Recv-Q  Send-Q  Local Address:Port   Peer Address:Port   Process
   tcp    LISTEN  0       128     0.0.0.0:22           0.0.0.0:*           users:(("sshd",pid=666,fd=3))
   tcp    LISTEN  0       511     0.0.0.0:80           0.0.0.0:*           users:(("nginx",pid=1234,fd=6))
   udp    UNCONN  0       0       0.0.0.0:68           0.0.0.0:*           users:(("dhclient",pid=567,fd=7))

  Расшифровка опций:
  | опция | расшифровка |
  |-------|-------------|
  | -t    | TCP-соединения |
  | -u    | UDP-соединения |
  | -l    | Listening (слушающие порты) |
  | -p    | Показать PID и имя процесса |
  | -n    | Не преобразовывать IP в имена (цифрами) |

  Что означают поля:
  | Поле | Значение |
  |------|----------|
  | Netid | Протокол (tcp, udp) |
  | State | Состояние (LISTEN — ждёт подключений) |
  | Local Address:Port | Какой порт слушает машина |
  | Peer Address:Port | С какой машины подключились (:* — любой) |
  | Process | Какой процесс и его PID | 

  Полезные команды:
  | Команда | Что показывает |
  |---------|----------------|
  | ss -t   | Только TCP-соединения |
  | ss -u   | Только UDP-соединения |
  | ss -l   | Только слушающие порты |
  | ss -t -a | Все TCP-соединения (включая установленные) |
  | ss -t -a -p | Показать PID процессов |
  | ss -t state established | Только установленные соединения | 

---

8. netstat - Классическая команда для просмотра сетевых соединений. В современных системах рекомендуется использовать ss
   ```bash
   netstat -tulpn

   $ sudo netstat -tulpn
   Active Internet connections (only servers)
   Proto Recv-Q Send-Q Local Address   Foreign Address   State    PID/Program name
   tcp   0      0      0.0.0.0:22      0.0.0.0:*         LISTEN   666/sshd
   tcp   0      0      0.0.0.0:80      0.0.0.0:*         LISTEN   1234/nginx
   udp   0      0      0.0.0.0:68      0.0.0.0:*                 567/dhclient

Полезные команды:
| Команда | Что показывает |
|---------|----------------|
| netstat -i | Статистика по сетевым интерфейсам |
| netstat -r | Таблица маршрутизации (как ip route) |
| netstat -s | Статистика по протоколам (TCP, UDP, ICMP) | 
