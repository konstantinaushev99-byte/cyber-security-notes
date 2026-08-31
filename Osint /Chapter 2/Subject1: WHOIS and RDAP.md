# WHOIS / RDAP
### Суть: Домен нельзя зарегистрировать анонимно на уровне протокола — при регистрации указываются данные регистранта, регистратор, даты
### WHOIS - это база данных этих записей, которую держат регистраторы и реестры доменных зон (например, Versign для .com)

## Что можно узнать:
1. Регистратор (GoDaddy, Namecheap и т.д.) — не владелец, а компания-посредник
2. Дата создания домена (Creation Date) и дата истечения (Expiration Date)
3. Дата последнего обновления записи
4. Иногда — имя, email, телефон, адрес владельца (если не скрыты)
5. Статус домена (clientTransferProhibited и т.п. — технические флаги)

# Почему данные владельца часто скрыты
С 2018 года из-за GDPR большинство регистраторов по умолчанию маскируют личные данные европейских (и не только) владельцев, подставляя вместо них данные privacy-прокси-сервиса. Так что на практике ты чаще увидишь "Privacy Protected" вместо реального имени

# RDAP, тот же смысл, что и WHOIS, но:
1. Ответ в JSON, а не свободном тексте - легче парсить программно
2. Стандартизированная схема полей ( у WHOIS формат отличается от регистратора к регистратору)
3. Постпенно становится основным протоколом, WHOIS не исчезает полностью, но RDAP - будущее

# Практика
```bash
1. whois tryhackme.com
или
2. rdap.org/domain/tryhackme.com
```

На что смотреть глазами OSINT-аналитика:
1. Домен создан вчера, а сайт выдает себя за банк с 20 летней историей? **КРАСНЫЙ ФЛАГ**
2. Регистратор из необычной юрисдикции для локального бизнеса - тоже повод присматрется
3. Не скрытые конктакты - не спеши дововерять на 100%, но это зацепка для обратного WHOIS

# Вывод WHOIS
```bash
Domain name: tryhackme.com
Registry Domain ID: 2282723194_DOMAIN_COM-VRSN - уникальный идентификатор в реестре .com 
Registrar WHOIS Server: whois.namecheap.com - домен зарегестрирован через NameCheap
Registrar URL: http://www.namecheap.com
Updated Date: 2025-05-11T14:06:03.00Z - последнее изменение записи
Creation Date: 2018-07-05T19:46:15.00Z - домену больше 7 лет
Registrar Registration Expiration Date: 2034-07-05T19:46:15.00Z - зарегистрирован сразу на много лет вперёд
Registrar: NAMECHEAP INC
Registrar IANA ID: 1068
Registrar Abuse Contact Email: abuse@namecheap.com
Registrar Abuse Contact Phone: +1.9854014545
Reseller: NAMECHEAP INC
Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited - технический флаг, поставленный регистратором, чтобы домен нельзя было случайно/злонамеренно перенести к другому регистратору без доп. подтверждения
Registry Registrant ID:
Registrant Name: Redacted for Privacy Purposes
Registrant Organization: Privacy service provided by WITHHELD FOR PRIVACY LLC
Registrant Street: 16192 Coastal Highway
Registrant City: Lewes
Registrant State/Province: Delaware
Registrant Postal Code: 19958
Registrant Country: US
Registrant Phone: +1.3022061391
Registrant Phone Ext:
Registrant Fax:
Registrant Fax Ext:
Registrant Email: a70a4ff6d25041a48378997194f9e834.protect@withheldforprivacy.com
Registry Admin ID:
Admin Name: Redacted for Privacy Purposes
Admin Organization: Privacy service provided by WITHHELD FOR PRIVACY LLC
Admin Street: 16192 Coastal Highway
Admin City: Lewes
Admin State/Province: Delaware
Admin Postal Code: 19958
Admin Country: US
Admin Phone: +1.3022061391
Admin Phone Ext:
Admin Fax:
Admin Fax Ext:
Admin Email: a70a4ff6d25041a48378997194f9e834.protect@withheldforprivacy.com
Registry Tech ID:
Tech Name: Redacted for Privacy Purposes
Tech Organization: Privacy service provided by WITHHELD FOR PRIVACY LLC
Tech Street: 16192 Coastal Highway
Tech City: Lewes
Tech State/Province: Delaware
Tech Postal Code: 19958
Tech Country: US
Tech Phone: +1.3022061391
Tech Phone Ext:
Tech Fax:
Tech Fax Ext:
Tech Email: a70a4ff6d25041a48378997194f9e834.protect@withheldforprivacy.com
Name Server: kip.ns.cloudflare.com 
Name Server: uma.ns.cloudflare.com
( Name Server: KIP.NS.CLOUDFLARE.COM / UMA.NS.CLOUDFLARE.COM:
1. DNS-зона обслуживается Cloudflare
2. Сайт почти наверняка проходит через Cloudflare proxy/CDN — то есть если мы сделаешь dig A tryhackme.com, получишь IP Cloudflare, а не реальный IP сервера, на котором физически крутится сайт
DNSSEC: unsigned - домен не использует DNSSEC
URL of the ICANN WHOIS Data Problem Reporting System: http://wdprs.internic.net/
>>> Last update of WHOIS database: 2026-08-30T11:24:07.34Z <<<
For more information on Whois status codes, please visit https://icann.org/epp
Registrant / Admin / Tech блоки
Заполнены все три роли одинаково:
- Redacted for Privacy Purposes — это NameCheap использует свой privacy-сервис WITHHELD FOR PRIVACY LLC
- Адрес 16192 Coastal Highway, Lewes, Delaware, 19958 — это не реальный адрес владельца TryHackMe, а юридический адрес самого privacy-сервиса
- Телефон +1.3022061391 — тоже сервисный, общий для всех клиентов privacy-защиты, не личный номер владельца.

Email — это самое важное поле здесь
a70a4ff6d25041a48378997194f9e834.protect@withheldforprivacy.com
Это алиас-адрес (proxy email), уникальный для конкретного домена в системе privacy-провайдера. Письмо, отправленное на этот адрес, форвардится реальному владельцу, но сам адрес не раскрывает, кто он. Хеш-подобная строка перед @ — это внутренний идентификатор privacy-сервиса для этой конкретной записи, не имеет отношения к владельцу напрямую.
```

# Rdap
Полный ответ: см. https://about.rdap.org/

Ключевые находки:
- Registrant/Technical entities — privacy-proxy (WithheldForPrivacy)
- Email — уникальный хеш-алиас, бесполезен для reverse WHOIS
- NS — Cloudflare → реальный IP сервера скрыт за прокси

# Итог

- Домен создан в 2018, зарегистрирован до 2034 → признак легитимного долгоживущего проекта
- Privacy-защита активна → deanon владельца через WHOIS/RDAP невозможен
- Cloudflare NS → для получения реального IP нужны другие методы (historical DNS, CT logs)
