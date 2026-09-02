# CMS ( Content Management System )
## готовая платформа для управления контентом без написания кода с нуля: WordPress, Drupal, Joomla, Shopify (для магазинов), Wix, Webflow

### Признаки WordPress
```bash
/wp-content/
/wp-includes/
/wp-json/
<meta name="generator" content="WordPress 6.4.2">
```
### Признаки Drupal:
```bash
/sites/default/
Drupal.settings
X-Generator: Drupal 10
```
### Признаки Shopify:
```bash
cdn.shopify.com в путях к ресурсам
Powered by Shopify (в футере, если не убрано)
```
### Полезный трюк — WordPress版本 через readme.html или feed:
```bash
site.com/readme.html
site.com/feed/ — RSS-фид часто содержит версию генератора
```
