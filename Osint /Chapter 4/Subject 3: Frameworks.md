# Frameworks
## Фреймворк — это то, на чём построено само приложение (backend: Django, Laravel, Express, Ruby on Rails; frontend: React, Vue, Angular, Next.js)

### Backend-фреймворки — признаки:
- Заголовок X-Powered-By: Express или X-Powered-By: PHP/8.2 - прямая подсказка
- Специфичные куки: Laravel ставит куку с именем вроде laravel_session, Django — csrftoken, Rails — _session_id
- Формат страниц ошибок в debug-режиме (если случайно оставлен включённым в продакшене) — у каждого фреймворка узнаваемый стиль трейсбека

### Frontend-фреймворки — признаки:
- Пути к статике: /_next/static/ — почти 100% Next.js, /static/js/ с хешами в имени файла — часто Create React App, ng-version атрибут в HTML — Angular
- View Source страницы — у React/Vue корневой <div id="root"> или <div id="app"> почти пустой, весь контент рендерится JS'ом (характерный признак SPA — Single Page Application)
