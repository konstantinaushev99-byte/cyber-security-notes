# Authentication bypass via information disclosure
## Цель: обойти аутентификацию, зайти в панель админа и удалить carlos

### Пути решения:
1. Тут я использовал метод TRACE
2. Сначало я пробавал имя Carlos и wiener, но это ничего не дало
3. Тогда я решил попробывать сделать это с admin
4. В Burp Repeater ввел TRACE /admin
5. И в самом низу можно увидеть заголовок который содержит наш ip
6. Если мы перейдем в Proxy > Match and replace, добавим match/replace rule, в Type поставим - Request header, а в   Replace - X-Custom-IP-Authorization: 127.0.0.1
7. И нажмем Test
8. И в разделе  Auto-modified request, увидим что burp добавил X-Custom-IP-Authorization заголовок к измененному запросу
9. Нажмем OK, теперь Burp Proxy будет добавлять X-Custom-IP-Authorizationзаголовок к каждому отправляемому вами запросу
10. Перейдем на главную страницу, и увидим что теперь нам доступна admin panel

Ссылка: https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-authentication-bypass
