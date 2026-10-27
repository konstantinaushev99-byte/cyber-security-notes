# Information disclosure on debug page
## Цель: Найти отладочную страницу, раскрывающую конфиденциальную информацию о приложении. 

### Пути решения:
1. Откроем любой продукт и посмотрим его исходный код
2. В конце кода можно увидеть что есть строка которую не видно при просмотре обычной страници
3. Вот этот кусок кода -                    </section>
                    <!-- <a href=/cgi-bin/phpinfo.php>Debug</a> -->
                </div>
4. Берем ссылку и возращаемся на главную страницу товаров, вставляем ее в ссылку - /cgi-bin/phpinfo.php
5. И мы попадем на страницу отладки
6. Тут мы долистаем до Additional Modules, и в нем найдем строку SECRET_KEY, котороя нам и нужна копируем значение Value, у меня оно - hgorhxeu0oa8gr544fvycf8zrano189p
7. И вставляем в ответ

Ссылка:  https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-on-debug-page
