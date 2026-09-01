# Search for public documents
## Суть: компании и люди регулярно случайно публикуют файлы, которые не должны быть в открытом доступе — через незащищённые директории, забытые ссылки, индексацию облачных хранилищ

# Ключевые dork-паттерны
## По типу файла + контексту:
```bash
site:example.com filetype:pdf
site:example.com filetype:doc OR filetype:docx
site:example.com filetype:xls OR filetype:xlsx
site:example.com filetype:pptx
```
Разные форматы дают разное: PDF — часто отчёты/презентации, XLS(X) — таблицы с данными (иногда финансы, списки сотрудников), DOC(X) — внутренние документы, договоры

## Открытые директории (open directory listing):
```bash
intitle:"index of" site:example.com
intitle:"index of" "parent directory" example.com
```
Это ситуация, когда веб-сервер настроен неправильно и вместо страницы отдаёт голый список файлов в папке — как проводник Windows, только в браузере. Частая находка на плохо настроенных серверах

## Поиск конфиденциальных ключевых слов в документах:
```bash
site:example.com filetype:pdf "confidential"
site:example.com filetype:xlsx "salary" OR "password"
```

## Облачные хранилища (не через site:, а напрямую):
```bash
site:docs.google.com "example.com" filetype:pdf
site:drive.google.com "example.com"
```
Компании часто расшаривают Google Docs/Drive-документы "по ссылке для всех, у кого есть ссылка" — если ссылка утекла (или Google их частично индексирует), можно найти

# Почему это работает
## Поисковик индексирует всё, что физически публично доступно по URL, независимо от того, хотел ли владелец, чтобы это нашли. Отсутствие ссылки на файл с главной страницы не защищает файл — если у него есть прямой URL и на него нет robots.txt/пароля, поисковый бот его найдёт и проиндексирует рано или поздно
