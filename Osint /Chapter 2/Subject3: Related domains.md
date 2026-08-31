# Related domains — практика

Проблема в том, что для tryhackme.com у нас нет контактов владельца (privacy защита в WHOIS/RDAP), так что классический Reverse WHOIS по email не сработает. 
Но у нас уже есть несколько зацепок из DNS/TXT, которые можно использовать как альтернативные векторы:
1. Через IP-инфраструктуру (Vercel). Раз сайт на Vercel — можно поискать, что ещё висит на тех же IP/подсетях (64.239.123.0/24, 64.239.109.0/24).
Правда, у Vercel это shared-хостинг для тысяч клиентов, так что результат может быть шумным (много несвязанных сайтов на тех же IP) — это стоит учитывать как ограничение метода
2. Через TXT-находки. Zapier, Chargebee, HubSpot — сами по себе не дадут доменов напрямую, но если бы у нас был доступ к их публичным партнёрским/кейс-стади страницам, иногда можно найти упоминания клиента.
3. Через SSL-сертификат (Certificate Transparency - Один и тот же сертификат иногда покрывает несколько доменов (SAN — Subject Alternative Names), особенно если компания использует wildcard или multi-domain сертификат

Заходим на crt.sh, и вводим: tryhackme.com или если сайт не работает то заходим на censys

# Метод
- Certificate Transparency logs через Censys (`cert.names: tryhackme.com`)
- Каждый TLS-сертификат логируется публично → раскрывает поддомены, даже неактивные/забытые
- Untrusted/expired сертификаты игнорировать по валидности, но само ИМЯ в них всё равно валидная находка

## Найденные поддомены

### Публичные
| Поддомен | Назначение |
|---|---|
| business.tryhackme.com | B2B-раздел |
| careers.tryhackme.com | Вакансии |
| resources.tryhackme.com | Учебные материалы |
| assets.tryhackme.com | Статика |
| store.tryhackme.com | Магазин |
| help.tryhackme.com | Саппорт |

### Внутренняя инфраструктура 
| Поддомен | Что раскрывает |
|---|---|
| auth.tryhackme.com | CNAME → workos-dns.com → используют WorkOS для SSO/auth |
| setup.auth.tryhackme.com | Онбординг auth-флоу |
| *.guacaworker.tryhackme.com | Apache Guacamole → браузерный доступ к VM (VNC/RDP через HTML5) |
| *.cell-prod-us-east-1d.vm.tryhackme.com | Cell-based architecture, AWS регион us-east-1d |
| ec2-dash.vm.tryhackme.com | Дашборд AWS EC2 |
| reverse-proxy-us-east-1.tryhackme.com | Reverse-proxy слой, тот же регион |
| costra.vm.tryhackme.com | VM-инфраструктура (назначение неясно) |

# Итог
- Облачный провайдер: **AWS**, регион **us-east-1**
- Архитектура: **cell-based** (изоляция инстансов по "ячейкам")
- Auth: сторонний провайдер **WorkOS**, не самописный
- Доступ к лабам: **Apache Guacamole** (браузерный VNC/RDP-шлюз)

# Инструманты которыми пользовался
- Censys (search.censys.io/certificates) — основной источник
- crt.sh — альтернатива (был недоступен в моменте, 502)
- dig — резолв найденных поддоменов, проверка CNAME
- whois на резолвнутый IP — определение хостинг-провайдера
