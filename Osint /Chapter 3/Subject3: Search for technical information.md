# Search for technical information
## Здесь фокус смещается с документов на инфраструктуру, код, конфигурацию сайта

## Поиск сообщений об ошибках и debug-информации
```bash
site:example.com "fatal error"
site:example.com "warning: mysql"
site:example.com intext:"stack trace"
```
Ошибки PHP/SQL, случайно показанные посетителю вместо аккуратной страницы 404, могут раскрыть путь на сервере, версию софта, структуру базы данных

## Поиск конфигурационных и служебных файлов
```bash
site:example.com filetype:env
site:example.com filetype:log
site:example.com filetype:sql
site:example.com filetype:bak
site:example.com ext:conf
```
Файлы .env, .log, .sql, .bak — это файлы, которые никогда не должны быть публично доступны (часто содержат пароли, API-ключи, дампы баз данных), но иногда оказываются на веб-сервере по невнимательности разработчика и попадают в индекс

## Поиск версий ПО и технологий
```bash
site:example.com "powered by"
site:example.com inurl:wp-content
site:example.com "WordPress" filetype:txt
```
inurl:wp-content сразу скажет "это WordPress" без единого захода на сайт. Зная CMS/фреймворк, дальше можно искать публично известные уязвимости именно этой версии (уже не OSINT, а следующий шаг — но разведка это позволяет спланировать)

# Поиск через специализированные поисковики (не просто Google/Bing)
- Shodan / Censys — индексируют не веб-страницы, а сами устройства/сервера по баннерам, портам, версиям сервисов
- GitHub search — site:github.com "example.com" password — поиск случайно закоммиченных секретов в публичных репозиториях
