# Practice
## Сделаем fingerprinting на github.com, чтобы увидить все на практике, а не на теории

### Шаг 1: HTTP-заголовки
В терминале:
```bash
cutl -I https://github.com
```
Разбор вывода: <img width="2527" height="690" alt="image" src="https://github.com/user-attachments/assets/7be9ef5f-ebea-4a9a-9a3e-68d535750e27" />
- Server: github.com
  - Видим что тут нет названия технологии сервера, а просто бренд. Это хороший пример намеренного сокрытия fingerprint'a, о котором я писал в теории
- HTTP/2 200
  - Маленькая деталь, котороя говорит что сайт работает по протоколу HTTTP/2
- content-security-policy(CSP)
  - CSP - это заголов безопасности, который говорит браузеру "Закгружать ресурсы только с этих доменов", и это отличный источник для  разведки: компания вынуждена перечислять каждый домен, с которым взаимодейстует
- Из моего вывода можно вынести:
  - *.blob.core.windows.net - GitHub использует Microsoft Azure Blob Storage для хранения части данных
  - *.s3.amazonaws.com - а также AWS S3 используется параллельно с Azure — то есть у GitHub мультиоблачная (multi-cloud) инфраструктура
  - api.visualstudio.com, *.rel.tunnels.api.visualstudio.com - интеграция с Visual Studio
  - copilot-proxy.githubusercontent.com, api.individual.githubcopilot.com, api.business.githubcopilot.com - инфраструктура GitHub Copilot, есть разделение на individual/business/enterprise прокси
  - fullstory.com - FullStory — это сервис аналитики поведения пользователей

# Итог 
Я только что своими глазами увидел, как CSP-заголовок, задуманный исключительно для безопасности (защита от XSS), случайно становится богатейшим источником OSINT — он вынужден перечислить буквально всю партнёрскую/облачную инфраструктуру компании.

    
