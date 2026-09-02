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

---

# следующий шаг: View Source страницы github.com, ищем meta-теги и признаки фреймворка/технологий в самом HTML. Ctrk+U на github.com

Разбор вывода:
- У нас нет <meta name="generator" content="...">, значит GitHub использует кастомную платформу без стандартного generation-тега, либо намеренно убрал его.
- Turba/Hotwire
  ```bash
  data-turbo-transient
  turbo-cache-control
  turbo-body-classes
  ```
  слово turba это прямая отсылка к Hotwire Turbo, JS-библиотеке от компании 37signals (создатели Ruby on Rails), которая
  используется для быстрых переходов между страницами без полной перезагрузки 
- x-pjax — легаси-слой:
  ```bash
  x-pjax-version, x-pjax-csp-version, x-pjax-css-version, x-pjax-js-version
  ```
  Тут у нас есть две технологии PJAX и Turbo, оба набора meta-тегов присутствуют одновременно, либо  GitHub находится в
  процессе миграции с PJAX на Turbo, либо держит PJAX-инфраструктуру для обратной совместимости со старым кодом
- octolytics-* — собственная аналитика:
  ```bash
  octolytics-url content="https://collector.github.com/github/collect"
  octolytics-actor-id, octolytics-actor-login, octolytics-actor-hash
  ```
  GitHub не использует стороннюю аналитику (Google Analytics и т.п.) для этого — у них собственная встроенная система под
  названием Octolytics, с собственным коллектором данных
- Скрытая находка — закодированный payload
  ```bash
  visitor-payload content="eyJyZWZlcnJlciI6bnVsbCwicmVxdWVzdF9pZCI6..."
  ```
  Это Base64-строка, если ее раскодировать, внутри окажится JSON с полями region_edge и region_render.

  ---

# Теперь сверим весь ручной разбор с автоматическим инструментом(Wappalyzer в браузере)
Разберем что нам дал Wappalyzer - <img width="498" height="550" alt="image" src="https://github.com/user-attachments/assets/42c24bd5-9cdc-475d-87c6-bac474154840" />
- Совпало с нашиими находками:
  - Turbo — Wappalyzer подтвердил именно то, что мы вычислили по meta-тегам
  - Amazon Web Services / Amazon S3 — подтверждает то, что мы нашли в CSP-заголовке
  - GitHub Pages — логично, раз домен github.com сам себя хостит через свою же платформу
  - WebSocket — подтверждает shared-web-socket ссылку (wss://alive.github.com/...), которую мы видели в meta-тегах
- Что мы не нашли ручным поиском
  - React + React Router 8.3.0 — а вот это важно!!!. Мы предполагали Rails/Turbo как основу (по meta-тегам), но Wappalyzer
    показывает, что frontend всё-таки построен на React поверх этого
  - lit-html 1.1.2 — ещё одна JS-библиотека для рендеринга, которую мы вообще не заметили
  - Tailwind CSS — CSS-фреймворк, полностью незаметный при простом просмотре HTML-кода
  - PWA (Progressive Web App) — GitHub можно "установить" как приложение
  - Priority Hints — техника оптимизации загрузки ресурсов, тонкая деталь, которую руками не увидеть вообще
 
  # Вывод по всей практике
  - Это наглядное доказательство того, зачем вообще нужны специализированные fingerprinting-инструменты вроде Wappalyzer:
    ручной анализ (curl + view-source) находит поверхностные вещи — заголовки, явные meta-теги, пути к файлам,
    автоматизированные инструменты находят скрытые технические сигнатуры, которые физически не видны в исходном коде без
    специального анализа.
