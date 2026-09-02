# Источник: Security Rules – CompTIA Network+ N10-009 – 4.3
# Security Rules

---

# ACL (Access Control List)
- ACL (Список контроля доступа) — это механизм, который позволяет определять, какой трафик разрешён, а какой запрещён в сети

Принцип работы ACL
- Большинство Firewall проверяют правила сверху вниз
  - Rule 1
  - Rule 2
  - Rule 3
- Как только найдено совпадение — применяется действие

---

Implicit Deny 
- Если пакет не подошёл ни под одно правило ACL, он автоматически блокируется(Implicit Deny (Неявный запрет))
- То есть правило может даже не существовать явно, но трафик всё равно будет отклонён

---

# Пример правил ACL
- Rule 1
  - Разрешить TCP порт 22 -> Allow TCP 22(SSH)
- Rule 2
  - Разрешить TCP порт 80 -> Allow TCP 80(HTTP)
- Rule 3
  - Разрешить TCP порт 443 -> Allow TCP 443(HTTPS)
- Rule 4
  - Разрешить TCP порт 3389 -> Allow TCP 3389(RDP)
- Rule 5
  - Разрешить UDP порт 53 -> Allow UDP 53(DNS)
- Rule 6
  - Разрешить UDP порт 123 -> Allow UDP 123(NTP)
- Rule 7
  - Запретить ICMP -> Deny ICMP(Это блокирует: Ping, Traceroute, другие ICMP-сообщения)
 
---

# URL Filtering
- URL Filtering позволяет контролировать доступ к сайтам
- Можно разрешать или блокировать:
  - отдельные URL
  - целые категории сайтов
- Пример:
  - Block: hacking websites
  - Allow: educational websites

---

# Content Filtering
- фильтрация содержимого трафика
  - Проверяется не адрес сайта, а сами данные
- Примеры:
  - Antivirus
  - Data Loss Prevention (DLP)(утечка данных)
  - Родительский контроль

---

# Security Zones
- Для упрощения правил Firewall используют зоны безопасности
- Примеры зон:
  - Trusted Zone(Доверенная сеть)
  - Untrusted Zone(Недоверенная сеть)
- Другие варианты:
  - Internal Zone
  - External Zone
  - Server Zone
  - Database Zone
  - DMZ Zone

  ---

  # Для собеседования
  - ACL — список правил, разрешающих или запрещающих трафик
  - Implicit Deny — всё, что не разрешено явно, запрещено
  - URL Filtering — фильтрация сайтов по URL или категории
  - Content Filtering — фильтрация содержимого данных
  - Screened Subnet (DMZ) — отдельная сеть для публичных сервисов
  - Security Zones — логические области сети для упрощения правил Firewall























