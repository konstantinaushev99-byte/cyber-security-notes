# DNS - записи практика

# Введем даные команды:
```bash
dig A tryhackme.com
dig AAAA tryhackme.com
dig NS tryhackme.com
dig MX tryhackme.com
dig TXT tryhackme.com
```

# Команды dig A tryhackme.com и  dig AAAA tryhackme.com
```bash
<img width="656" height="761" alt="image" src="https://github.com/user-attachments/assets/5865fe44-9ba8-44dd-90f5-2b13ee35095b" />
Структура ответа dig — 4 секции
1. QUESTION SECTION — что я спросил (A-запись tryhackme.com)
2. ANSWER SECTION — вот это настоящий ip сайта:
tryhackme.com. 113 IN A 64.239.123.65
tryhackme.com. 113 IN A 64.239.109.65
 ВАЖНО: наличие Cloudflare в NS (AUTHORITY) не означает,
   что IP в ANSWER тоже будет Cloudflare — это зависит от того,
   включен ли proxy (orange cloud) для конкретной записи.
   Cloudflare может быть только DNS-провайдером (NS),
   а сам сайт может стоять на любом другом хостинге напрямую
3. AUTHORITY SECTION — кто авторитетный сервер для этой зоны (NS-записи Cloudflare), просто подтверждение, откуда пришёл ответ
4. ADDITIONAL SECTION — это IP-адреса самих NS-серверов (kip.ns.cloudflare.com, uma.ns.cloudflare.com), а не IP сайта tryhackme.com! Это называется glue records — вспомогательная информация, чтобы резолвер не делал ещё один DNS-запрос для поиска IP самих нейм-серверов
```

# Вывод команд dig NS tryhackme.com dig MT tryhackme.com dig TXT tryhackme.com
```bash
dig NS tryhackme.com - <img width="703" height="709" alt="image" src="https://github.com/user-attachments/assets/0aabce9e-2835-4a9a-96a9-75e1a1109542" />
NS потвердила то, что мы уже знали - kip.ns.cloudflare.com/uma.ns.cloudflare. Контрольная проверка пройдена.

dig MX tryhackme.com - <img width="699" height="846" alt="image" src="https://github.com/user-attachments/assets/a76b3575-8300-4040-90a2-2d44d5b86183" />
MX видим класический патерн Google Workspace: одна основная запись с приоритетом 1 (чем меньше число, тем выше приоритет - почта сначала пытается уйти туда),
и 4 резервных alt-сервера с пониающимся приоритетом на случай недоступности основого. Это стндартный, "Книжный" вид  MX для Google Workspace

dig TXT tryhackme.com - <img width="1397" height="908" alt="image" src="https://github.com/user-attachments/assets/bdc7766c-ff5b-490d-9b6b-bcdedc36c903" />
TXT SPF: v=spf1 include:_spf.google.com include:email.chargebee.com include:7168674.spf05.hubspotemail.net ~all
Это раскрывает сразу 3 сервиса, имеющих право слать почту от имени tryhackme.com:
1. _spf.google.com - подверждает Google Worckspace (совподает с MX)
2. email.chargebee.com - Chargebee, платформа биллинга/подписок. Значит tryhackme использует Chargebee для обработки платежей/подписок (счет,инвойсы, уведомление об оплате идут через него)
3. hubspotemail.net - HubSpot, используется для  маркетинговых/транзакционных рассылок(email-маркетинг, CRM)

Verification-токены (не дают доступа, просто подтверждают владение доменом для сторонних сервисов):
- google-site-verification (целых 3 штуки!) — значит верифицировано несколько разных Google-продуктов/аккаунтов под этот домен
- zapier-domain-verification — используют Zapier для автоматизации workflow между сервисами
- anthropic-domain-verification — любопытно: значит домен верифицирован в системах Anthropic (вероятно, для доступа к Claude for Work/Enterprise-аккаунту или API-интеграции на уровне организации)
- apple-domain-verification — верификация для сервисов Apple (может быть Apple Business Manager, Sign in with Apple, или push-уведомления)
```

# Итог
Из одной TXT-записи корневого домена мы, по сути, востановили тех-стек компании: Google Workspace(почта), Chargebee(биллинг), HubSpot(маркетинг), Zapier(автоматизация), плюс факт использования Anthropic и Apple сервисов. 
Это классический пример ,  как пассивная OSINT-разведка через DNS раскрывает бизнес-инфраструктуру без единого активного действия против самого сайта. 
