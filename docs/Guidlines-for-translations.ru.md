# Руководство по переводу

## Как переводить

- Индексные страницы находятся в папке docs, английская версия - ``index.md``, если мы хотим перевести страницу на другой язык, нам нужно назвать ее ``index.<код языка>.md``. Например, index.ru.md, правильные коды находятся в mkdocs.md 
  Изображения книг должны быть созданы в папке mkdocs с кодом языка Например: nl, fr, ru
- Перевод меню можно сделать в mkdocs.yml. Создайте параметр nav_translations: и переведите структуру меню.
  Пример:
```
- locale: nl
  name: Nederlands
  build: true
  nav_translations:
    Welcome: Welkom
    Getting started: Aan de slag
    The basics: De basis
    Problem detection: Probleem detectie
```
- Если вы хотите перевести страницу, создайте файл с тем же именем, что и оригинал, но добавьте код страны перед .md Например: Requirements.md станет Requirements.ru.md. Если вы не делаете скриншоты с переводом, то используйте те же изображения в папке image.
- Если вы хотите добавлять новые скриншоты, создайте новую папку image.<код языка> Например: image/ станет image.ru/

## Как написать тему

- Все пишется в формате markdown, HTML поддерживается, но старайтесь избегать его, если он не нужен
- Начните страницу с заголовка #
- Когда пишете тему - начните с введения, о чем эта тема
- Дайте обзор тем, которые будете рассматривать
- Добавьте наглядные изображения, если возможно, это лучше объясняет людям, как все работает
- Создайте инструкцию, чтобы люди увидели, как это сделать 
- поместите изображения в папку /image 
    - можете использовать английские скриншоты в папке image или создать свои собственные скриншоты на своём языке, используя /image.<код языка>, например: /image.ru
- Закройте каждую тему с помощью `---`, это нарисует горизонтальную линию 


## Таблица переводов 

- Эта таблица используется для отслеживания того, что было завершено и готово к переводу. Для этого мы помечаем страницу флажком :white_check_mark:. 
- Как только другие языки будут переведены, мы помечаем их :checkered_flag:
- В случае если страница обновляется новой информацией после того, как страницы уже переведены, мы помечаем ее :material-refresh: и снимаем флаг :checkered_flag: с переведенных страниц.

---

- Веб-страница готова к переводу: :white_check_mark:
- Веб-страница переведена : :checkered_flag:
- Веб-страница была обновлена после перевода: :material-refresh:
- Веб-страница еще не закончена : :construction:


???+ Note
    Пожалуйста, не обновляйте эту таблицу, она предназначена только для справки при переводе. Я буду поддерживать эту таблицу, когда буду добавлять новые темы или объединять изменения.


| Webpage			| English          | French           | German           | Italian | Dutch | Portugese | Spanish | Thai | Chinese |
|:----				| ----	           | ----             | ----             | ----    | ----  | ----      | ----    |----  | ----    |
|Welcome			|:white_check_mark:|                  |                  |         |       |           |         |      |         |
|				|                  |                  |                  |         |       |           |         |      |         |
|Getting started                |         	   |                  |                  |         |       |           |         |      |         |
|Requirements      		|:white_check_mark:|                  |                  |         |       |           |         |      |         |
|Installing Zabbix DB Server	|:white_check_mark:|                  |                  |         |       |           |         |      |         |
|Installing Zabbix		|:white_check_mark:|                  |                  |         |       |           |         |      |         |
|Configure Zabbix HA		|:white_check_mark:|                  |                  |         |       |           |         |      |         |
|				|                  |                  |                  |         |       |           |         |      |         |
|The basics			|                  |                  |                  |         |       |           |         |      |         |
|Zabbix Interface		|:white_check_mark:|                  |                  |         |       |           |         |      |         |
|Zabbix Users & User groups	|:white_check_mark:|                  |                  |         |       |           |         |      |         |
|Zabbix hosts			|:construction:    |                  |                  |         |       |           |         |      |         |
|Host groups			|:white_check_mark:|                  |                  |         |       |           |         |      |         |
|Interfaces			|:construction:    |                  |                  |         |       |           |         |      |         |
|templates			|                  |                  |                  |         |       |           |         |      |         |
|Items				|                  |                  |                  |         |       |           |         |      |         |
|Zabbix triggers		|                  |                  |                  |         |       |           |         |      |         |
|Macros				|:white_check_mark:|:white_check_mark:|                  |         |       |           |         |      |         |
|Data Flow			|                  |                  |                  |         |       |           |         |      |         |
|				|                  |                  |                  |         |       |           |         |      |         |
|Data collection		|                  |                  |                  |         |       |           |         |      |         |
|Zabbix Agent			|:construction:    |                  |                  |         |       |           |         |      |         |
|				|                  |                  |                  |         |       |           |         |      |         |
|Problem detection		|                  |                  |                  |         |       |           |         |      |         |
|Triggers			|                  |                  |                  |         |       |           |         |      |         |
|				|                  |                  |                  |         |       |           |         |      |         |
|Taking action when problems come|                 |                  |                  |         |       |           |         |      |         |
|Event based Actions		|                  |                  |                  |         |       |           |         |      |         |
|				|                  |                  |                  |         |       |           |         |      |         |
|Managing permissions		|                  |                  |                  |         |       |           |         |      |         |
|Managing Permissions		|                  |                  |                  |         |       |           |         |      |         |
|				|                  |                  |                  |         |       |           |         |      |         |
|Visualising Problems		|                  |                  |                  |         |       |           |         |      |         |
|Visualising our problems	|                  |                  |                  |         |       |           |         |      |         |
|				|                  |                  |                  |         |       |           |         |      |         |
|Automating configuration	|                  |                  |                  |         |       |           |         |      |         |
|Automating configuration	|                  |                  |                  |         |       |           |         |      |         |
|				|                  |                  |                  |         |       |           |         |      |         |
|VMWare monitoring		|                  |                  |                  |         |       |           |         |      |         |
|VMware monitoring with Zabbix	|                  |                  |                  |         |       |           |         |      |         |
|				|                  |                  |                  |         |       |           |         |      |         |
|Monitoring websites		|                  |                  |                  |         |       |           |         |      |         |
|Monitoring websites		|                  |                  |                  |         |       |           |         |      |         |
|				|                  |                  |                  |         |       |           |         |      |         |
|Monitoring SNMP, IPMI and JAVA	|                  |                  |                  |         |       |           |         |      |         |
|SNMP Monitoring		|                  |                  |                  |         |       |           |         |      |         |
|SNMP trap monitoring		|:construction:    |                  |                  |         |       |           |         |      |         |
|JAVA monitoring		|                  |                  |                  |         |       |           |         |      |         |
|IPMI Monitoring		|:construction:    |                  |                  |         |       |           |         |      |         |
|				|                  |                  |                  |         |       |           |         |      |         |
|Authentication			|                  |                  |                  |         |       |           |         |      |         |
|Authentication with HTTP	|                  |                  |                  |         |       |           |         |      |         |
|Authentication with LDAP	|                  |                  |                  |         |       |           |         |      |         |
|Authentication with SAML	|                  |                  |                  |         |       |           |         |      |         |
|Zabbix MFA support		|:white_check_mark:|                  |                  |         |       |           |         |      |         |
|				|                  |                  |                  |         |       |           |         |      |         |
|Monitoring with Proxies	|                  |                  |                  |         |       |           |         |      |         |
|Installing Proxies		|                  |                  |                  |         |       |           |         |      |         |
|Active proxy			|                  |                  |                  |         |       |           |         |      |         |
|Passive proxy			|                  |                  |                  |         |       |           |         |      |         |
|Proxy loadbalancing		|:white_check_mark:|                  |                  |         |       |           |         |      |         |
|				|                  |                  |                  |         |       |           |         |      |         |
|Securing Zabbix		|                  |                  |                  |         |       |           |         |      |         |
|Securing Zabbix Frontend	|:construction:    |                  |                  |         |       |           |         |      |         |
|Securing Zabbix with SELinux	|                  |                  |                  |         |       |           |         |      |         |
|				|                  |                  |                  |         |       |           |         |      |         |
|Maintaining Zabbix		|                  |                  |                  |         |       |           |         |      |         |
|Maintaining Zabbix		|                  |                  |                  |         |       |           |         |      |         |
|				|                  |                  |                  |         |       |           |         |      |         |
|Monitoring Windows		|                  |                  |                  |         |       |           |         |      |         |
|Monitoring Windows		|                  |                  |                  |         |       |           |         |      |         |
|				|                  |                  |                  |         |       |           |         |      |         |
|Zabbix API			|                  |                  |                  |         |       |           |         |      |         |
|Zabbix API			|                  |                  |                  |         |       |           |         |      |         |
|                               |                  |                  |                  |         |       |           |         |      |         |
|Zabbix extras                  |                  |                  |                  |         |       |           |         |      |         |
|Modbus monitoring with Zabbix  |:white_check_mark:|                  |                  |         |       |           |         |      |         |
|                               |                  |                  |                  |         |       |           |         |      |         |
