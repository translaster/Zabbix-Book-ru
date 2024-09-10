# Установка сервера баз данных Zabbix

В этой главе мы установим нашу базу данных Zabbix. Zabbix поддерживает широкий спектр баз данных SQL, но пока мы ограничимся PostgreSQL, MariaDB и MySQL.
Также поддерживается Oracle, но поддержка Oracle прекращена, Zabbix 7 будет последней версией Zabbix, поддерживающей Oracle в качестве бэкенда.
В этой главе мы расскажем, как установить и настроить каждую базу данных на отдельной машине, но вы также можете установить ее на свой сервер Zabbix. Нет никакого правила, которое заставляло бы вас устанавливать БД на свой сервер Zabbix или на отдельный сервер.

Для большинства установок, вероятно, будет достаточно БД на локальной машине, просто убедитесь, что БД находится на других дисках, а не в ОС. Если впоследствии производительность станет проблемой, то вы сможете перенести БД на другой сервер. Не забывайте, что локальные соединения быстрее соединений по TCP, так что не всегда лучше переносить БД на собственный сервер - все зависит от ваших потребностей.


## Установка Zabbix с MariaDB


Начнем с установки сервера MariaDB, для этого необходимо вручную создать файл конфигурации репозитория MariaDB `mariadb.repo` по следующему пути `/etc/yum.repos.d/`.
Чтобы создать файл репозитория MariaDB, вы можете использовать следующую команду.


### Добавление репозитория MariaDB

```
# vi /etc/yum.repos.d/mariadb.repo
```

Приведенная выше команда создаст новый файл репозитория, после создания которого необходимо добавить в него следующую конфигурацию.
Убедитесь, что ваша версия, в данном случае 10.11, поддерживается Zabbix, посмотрев последние [требования](https://www.zabbix.com/documentation/current/en/manual/installation/requirements) для вашей версии. 

```
# MariaDB 10.11 RedHatEnterpriseLinux repository list - created 2023-11-01 14:20 UTC
# https://mariadb.org/download/
[mariadb]
name = MariaDB
# rpm.mariadb.org is a dynamic mirror if your preferred mirror goes offline. See https://mariadb.org/mirrorbits/ for details.
# baseurl = https://rpm.mariadb.org/10.11/rhel/$releasever/$basearch
baseurl = https://mirror.23m.com/mariadb/yum/10.11/rhel/$releasever/$basearch
# gpgkey = https://rpm.mariadb.org/RPM-GPG-KEY-MariaDB
gpgkey = https://mirror.23m.com/mariadb/yum/RPM-GPG-KEY-MariaDB
gpgcheck = 1


```
Сначала обновите нашу ОС с помощью последних патчей.

```
# dnf update -y
```
### Установка базы данных MariaDB

Теперь мы готовы установить нашу базу данных MariaDB.

```
# dnf install MariaDB-server MariaDB-client
```

Теперь мы готовы запустить базу данных MariaDB и добавить её в автозапуск.

```
# systemctl enable mariadb --now
```

После завершения установки вы можете проверить версию сервера MariaDB с помощью следующей команды:

```
# mysql -V
```

Результат должен выглядеть следующим образом:

```
mysql  Ver 15.1 Distrib 10.11.6-MariaDB, for Linux (x86_64) using  EditLine wrapper
```

И когда мы спросим о состоянии нашего сервера MariaDB, мы должны получить вот такой результат:

```
# systemctl status mariadb

● mariadb.service - MariaDB 10.11.6 database server
     Loaded: loaded (/usr/lib/systemd/system/mariadb.service; enabled; preset: disabled)
    Drop-In: /etc/systemd/system/mariadb.service.d
             └─migrated-from-my.cnf-settings.conf
     Active: active (running) since Sat 2023-11-18 19:19:36 CET; 2min 13s ago
       Docs: man:mariadbd(8)
             https://mariadb.com/kb/en/library/systemd/
    Process: 41986 ExecStartPre=/bin/sh -c systemctl unset-environment _WSREP_START_POSITION (code=exited, status=0/SUCCESS)
    Process: 41987 ExecStartPre=/bin/sh -c [ ! -e /usr/bin/galera_recovery ] && VAR= ||   VAR=`cd /usr/bin/..; /usr/bin/galera_recovery`; [ $? -eq 0 ]   && systemctl set-environment _WSREP_START>
    Process: 42006 ExecStartPost=/bin/sh -c systemctl unset-environment _WSREP_START_POSITION (code=exited, status=0/SUCCESS)
   Main PID: 41995 (mariadbd)
     Status: "Taking your SQL requests now..."
      Tasks: 9 (limit: 12344)
     Memory: 206.8M
        CPU: 187ms


```
### Безопасность базы данных MariaDB

Пришло время защитить нашу базу данных, удалив тестовую базу и пользователя и установив собственный пароль root. Выполните команду `mariadb-secure-installation`, вы должны получить следующий результат.

```
# mariadb-secure-installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none):
OK, successfully used password, moving on...

Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.

You already have your root account protected, so you can safely answer 'n'.

Switch to unix_socket authentication [Y/n] n
 ... skipping.

You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] y
New password:
Re-enter new password:
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```
### Создание базы данных Zabbix

```
# mysql -uroot -p
password

MariaDB [(none)]> CREATE DATABASE zabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
MariaDB [(none)]> CREATE USER 'zabbix-web'@'<zabbix server ip>' IDENTIFIED BY '<password>';
MariaDB [(none)]> CREATE USER 'zabbix-srv'@'<zabbix server ip>' IDENTIFIED BY '<password>';
MariaDB [(none)]> GRANT ALL PRIVILEGES ON zabbix.* TO 'zabbix-srv'@'<zabbix server ip>';
MariaDB [(none)]> GRANT SELECT, UPDATE, DELETE, INSERT ON zabbix.* TO 'zabbix-web'@'<zabbix server ip>';
MariaDB [(none)]> SET GLOBAL log_bin_trust_function_creators = 1;
MariaDB [(none)]> QUIT

```

???+ warning 
    "В документации Zabbix прямо говорится о том, что детерминированные триггеры должны быть созданы во время импорта схемы. В MySQL и MariaDB для этого необходимо установить GLOBAL log_bin_trust_function_creators = 1, если включено двоичное протоколирование и нет привилегий суперпользователя, а log_bin_trust_function_creators = 1 не установлен в конфигурационном файле MySQL."


### Добавление репозитория Zabbix и заполнение DB

```
# rpm -Uvh https://repo.zabbix.com/zabbix/6.5/rocky/9/x86_64/zabbix-release-6.5-2.el9.noarch.rpm
# dnf clean all
# dnf install zabbix-sql-scripts
```

Загрузите данные из zabbix (структуру базы данных, снимки, пользователей, ... )

```
# zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uroot -p zabbix
```

???+ warning
    "В зависимости от скорости вашего оборудования или виртуальной машины это может занять от нескольких секунд до нескольких минут, поэтому, пожалуйста, не отменяйте работу, просто сидите и ждите подсказки."

Войдите в базу данных MariaDB под именем root

```
# mysql -uroot -p
```

Удалите глобальный параметр, поскольку он больше не нужен, а также из соображений безопасности.

```
MariaDB [(none)]> SET GLOBAL log_bin_trust_function_creators = 0;
Query OK, 0 rows affected (0.001 sec)
```

### Настройка брандмауэра

Последнее, что нам нужно сделать, это открыть брандмауэр и разрешить входящие соединения для базы данных MariaDB с нашего сервера Zabbix, потому что в данный момент мы не принимаем никаких соединений.

```
# firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources:
  services: cockpit dhcpv6-client  ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

Сначала мы создадим соответствующую зону для нашего MariaDB и откроем порт 3306/tcp, но только для ip нашего сервера Zabbix.

```
# firewall-cmd --new-zone=mariadb-access --permanent
success

# firewall-cmd --reload
success

# firewall-cmd --get-zones
block dmz drop external home internal mariadb-access nm-shared public trusted work

# firewall-cmd --zone=mariadb-access --add-source=<zabbix-serverip> --permanent

success
# firewall-cmd --zone=mariadb-access --add-port=3306/tcp  --permanent

success
# firewall-cmd --reload
```

Теперь давайте посмотрим на наши правила брандмауэра, чтобы убедиться, что они соответствуют нашим ожиданиям:

```
# firewall-cmd --zone=mariadb-access --list-all
```

```
mariadb-access (active)
  target: default
  icmp-block-inversion: no
  interfaces:
  sources: <ip from zabbix-server>
  services:
  ports: 3306/tcp
  protocols:
  forward: no
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

Теперь наш сервер базы данных готов принимать соединения от нашего сервера Zabbix :).
Вы можете продолжить выполнение следующей задачи [Установка сервера Zabbix](installing-zabbix.ru.md)

---

## Установка Zabbix с MySQL


Начнем с установки сервера MySQL. Сначала необходимо создать репозиторий MySQL, чтобы мы могли установить нужные файлы для нашего сервера MySQL.
Лучше всего проверить [документацию Zabbix](https://www.zabbix.com/documentation/current/en/manual/installation/requirements), чтобы узнать, какая версия поддерживается, чтобы не установить неподдерживаемую версию.

### Добавление репозитория MySQL

Выполните следующую команду, чтобы установить репозиторий MySQL для версии 8.0

```# dnf -y install https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm```

???+ note
    "Если вы устанавливаете эту систему на RedHat 8 и выше или альтернативные версии, такие как CentOS, Rocky или Alma 8, то вам нужно отключить модуль mysql, выполнив команду 'module disable mysql'."

Давайте сначала обновим нашу ОС с помощью последних патчей

```# dnf update -y```

#### Установка базы данных MySQL

```# dnf -y install mysql-community-server ```

Теперь мы готовы запустить базу данных MySQL и добавить её в автозапуск.

```# systemctl enable mysqld --now```

После завершения установки вы можете проверить версию сервера MySQL с помощью следующей команды:

```# mysql -V```

Результат должен выглядеть примерно так:

```mysql  Ver 8.0.35 for Linux on x86_64 (MySQL Community Server - GPL)```

И когда мы спросим о состоянии нашего сервера MariaDB, то должны получить вот такой результат:

```
# systemctl status mysqld

● mysqld.service - MySQL Server
     Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; preset: disabled)
     Active: active (running) since Mon 2023-11-20 22:15:51 CET; 1min 15s ago
       Docs: man:mysqld(8)
             http://dev.mysql.com/doc/refman/en/using-systemd.html
    Process: 44947 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
   Main PID: 45012 (mysqld)
     Status: "Server is operational"
      Tasks: 37 (limit: 12344)
     Memory: 448.3M
        CPU: 4.073s
     CGroup: /system.slice/mysqld.service
             └─45012 /usr/sbin/mysqld

Nov 20 22:15:43 mysql-db systemd[1]: Starting MySQL Server...
Nov 20 22:15:51 mysql-db systemd[1]: Started MySQL Server.
```
### Безопасность базы данных MySQL

MySQL защитит нашу базу данных случайным паролем root, который генерируется при установке базы данных. Первое, что нам нужно сделать, это заменить его на наш собственный пароль. Чтобы узнать пароль, нам нужно прочитать файл журнала с помощью следующей команды:

```# grep 'temporary password' /var/log/mysqld.log```

Как можно скорее измените пароль root, войдя в систему со сгенерированным временным паролем, и установите собственный пароль для учетной записи суперпользователя:
```
# mysql -uroot -p
```
```
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '<my mysql password>';
mysql> quit
```
Далее мы можем выполнить команду mysql_secure_installation, вы должны получить следующий результат:

???+ note
    "Повторно сбрасывать пароль root для MySQL не нужно, так как мы его уже сбросили. Следующий шаг необязателен, но рекомендуется."

```
# mysql_secure_installation

Securing the MySQL server deployment.

Enter password for user root:
The 'validate_password' component is installed on the server.
The subsequent steps will run with the existing configuration
of the component.
Using existing password for root.

Estimated strength of the password: 100
Change the password for root ? ((Press y|Y for Yes, any other key for No) : n

 ... skipping.
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
Success.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y
Success.

By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y
 - Dropping test database...
Success.

 - Removing privileges on test database...
Success.

Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
Success.

All done!
```

Давайте создадим пользователей БД и назначим им правильные разрешения в базе данных:

```mysql -uroot -p```

```
mysql> CREATE DATABASE zabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
mysql> CREATE USER 'zabbix-web'@'<zabbix server ip>' IDENTIFIED BY '<password>';
mysql> CREATE USER 'zabbix-srv'@'<zabbix server ip>' IDENTIFIED BY '<password>';
mysql> GRANT ALL PRIVILEGES ON zabbix.* TO 'zabbix-srv'@'<zabbix server ip>';
mysql> GRANT SELECT, UPDATE, DELETE, INSERT ON zabbix.* TO 'zabbix-web'@'<zabbix server ip>';
mysql> SET GLOBAL log_bin_trust_function_creators = 1;
mysql> QUIT
```

???+ warning
    "В документации Zabbix прямо говорится о том, что детерминированные триггеры должны быть созданы во время импорта схемы. В MySQL и MariaDB для этого необходимо установить GLOBAL log_bin_trust_function_creators = 1, если включено двоичное протоколирование и нет привилегий суперпользователя, а log_bin_trust_function_creators = 1 не установлен в конфигурационном файле MySQL."

### Добавление рептозитория Zabbix и заполнение базы данных

```
# rpm -Uvh https://repo.zabbix.com/zabbix/6.5/rocky/9/x86_64/zabbix-release-6.5-2.el9.noarch.rpm
# dnf clean all
# dnf install zabbix-sql-scripts

```
Теперь давайте загрузим данные из zabbix (структуру базы данных, образы, пользователей, ... ).

```
# zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uroot -p zabbix
Enter password:
```

???+ warning
    "В зависимости от скорости вашего оборудования или виртуальной машины это может занять от нескольких секунд до нескольких минут, поэтому, пожалуйста, не отменяйте работу, просто сидите и ждите подсказки."

```
Log back into your MySQL Database as root

# mysql -uroot -p
```

Удалите глобальный параметр, поскольку он больше не нужен, а также из соображений безопасности.

```
mysql> SET GLOBAL log_bin_trust_function_creators = 0;
Query OK, 0 rows affected, 1 warning (0.00 sec)
```

### Настройка брандмауера

И последнее, что нам нужно сделать, это открыть брандмауэр и разрешить входящие соединения с нашего сервера Zabbix к нашей базе данных MySQL, потому что на данный момент мы пока не принимаем никаких соединений.

```
# firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources:
  services: cockpit dhcpv6-client  ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

Сначала мы создадим соответствующую зону для нашей базы данных MySQL и откроем порт 3306/tcp, но только для IP-адреса нашего сервера Zabbix. Таким образом, никто из посторонних не сможет подключиться.

```
# firewall-cmd --new-zone=mysql-access --permanent
success

# firewall-cmd --reload
success

# firewall-cmd --get-zones
block dmz drop external home internal mysql-access nm-shared public trusted work

# firewall-cmd --zone=mysql-access --add-source=<zabbix-serverip> --permanent

success
# firewall-cmd --zone=mysql-access --add-port=3306/tcp  --permanent

success
# firewall-cmd --reload
```

Теперь давайте посмотрим на наши правила брандмауэра, чтобы убедиться, что они соответствуют нашим ожиданиям:

```
# firewall-cmd --list-all --zone=mysql-access
```

```
mysql-access (active)
  target: default
  icmp-block-inversion: no
  interfaces:
  sources: <ip from the zabbix-server>
  services:
  ports: 3306/tcp
  protocols:
  forward: no
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

Теперь наш сервер базы данных готов принимать соединения от нашего сервера Zabbix :).
Вы можете продолжить выполнение следующей задачи [Установка сервера Zabbix](installing-zabbix.ru.md)

---

## Установка Zabbix с PostgreSQL

Для настройки БД с PostgreSQL нам нужно сначала добавить в систему репозиторий PostgreSQL. На данный момент поддерживаются PostgreSQL 13-16, но лучше всего посмотреть перед установкой, так как новые версии могут поддерживаться, а старые - нет как самим Zabbix, так и PostgreSQL. Обычно лучше выбрать последнюю версию, которая поддерживается Zabbix. Zabbix также поддерживает расширение TimescaleDB, о котором мы поговорим позже. Как вы увидите, настройка PostgreSQL сильно отличается от MySQL не только установкой, но и защитой БД.

Таблицу совместимости можно найти [здесь](https://docs.timescale.com/self-hosted/latest/upgrades/upgrade-pg/).

### Добавление репозитория PostgreSQL

Итак, давайте сначала настроим наш репозиторий PostgreSQL с помощью следующих команд.

```
# Install the repository RPM:
sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm

# Disable the built-in PostgreSQL module:
sudo dnf -qy module disable postgresql

# Install PostgreSQL:
sudo dnf install -y postgresql16-server

# Initialize the database and enable automatic start:
sudo /usr/pgsql-16/bin/postgresql-16-setup initdb
sudo systemctl enable postgresql-16 --now
```

### Безопасность базы данных PostgreSQL

Как я уже говорил, PostgreSQL работает немного иначе, чем MySQL или MariaDB, и это относится и к тому, как мы управляем правами доступа. Postgres работает с файлом с именем pg_hba.conf, в котором мы должны указать, кто и откуда может получить доступ к нашей базе данных и какое шифрование используется для пароля. Итак, давайте отредактируем этот файл, чтобы разрешить нашему фронтенду и серверу zabbix доступ к базе данных.

???+ note
    "Аутентификация клиента настраивается в конфигурационном файле с именем ``pg_hba.conf``. HBA здесь означает аутентификацию на основе хоста. За дополнительной информацией обращайтесь к [документации PostgreSQL](https://www.postgresql.org/docs/current/auth-pg-hba-conf.html)."

Добавьте следующие строки, порядок которых важен.

```
# vi /var/lib/pgsql/16/data/pg_hba.conf
```

```
# "local" is for Unix domain socket connections only
local   zabbix          zabbix-srv                              	scram-sha-256
local   all             all                                     	peer
# IPv4 local connections:
host    zabbix          zabbix-srv      <ip from zabbix server/24>	scram-sha-256
host    zabbix          zabbix-web      <ip from zabbix server/24>	scram-sha-256
host    all             all             127.0.0.1/32            	scram-sha-256
```
После того как мы изменили файл pg_hba, не забудьте перезапустить postgres, иначе настройки не будут применены. Но перед перезапуском давайте также отредактируем файл postgresql.conf и разрешим нашей базе данных прослушивать сетевой интерфейс на предмет входящих соединений с сервера zabbix. Стандартно Postgresql будет разрешать соединения только из сокета.

```
# vi /var/lib/pgsql/16/data/postgresql.conf
```

и замените строку с listen_addresses, чтобы PostgreSQL прослушивал все интерфейсы, а не только наш localhost.

```
#listen_addresses = 'localhost' на  listen_addresses = '*'
```

После этого перезапустите кластер PostgreSQL и посмотрите, восстановится ли он в случае ошибки, проверьте файл ``pg_hba.conf``, который вы только что отредактировали, на наличие опечаток.

```
# systemctl restart postgresql-16
```

Для нашего сервера Zabbix нам нужно создать таблицы в базе данных, для этого нужно установить репозиторий Zabbix, как мы это делали для сервера Zabbix, и установить пакет Zabbix, содержащий все образы таблиц, базы данных, иконки, .....

### Добавление репозитория Zabbix и заполнение базы данных

```
# dnf install https://repo.zabbix.com/zabbix/6.0/rhel/9/x86_64/zabbix-release-6.0-4.el9.noarch.rpm -y
# dnf install zabbix-sql-scripts -y
```

Теперь мы готовы создать пользователей Zabbix для сервера и фронтенда:

```
# su - postgres 
# createuser --pwprompt zabbix-srv
Enter password for new role: <server-password>
Enter it again: <server-password>
``` 

Давайте сделаем то же самое для нашего фронтенда, создадим пользователя для подключения к базе данных:

```
# createuser --pwprompt zabbix-web
Enter password for new role: <frontend-password>
Enter it again: <frontend-password>
```

Далее нам нужно разархивировать файлы схемы базы данных. Выполните от имени пользователя root следующую команду:

```
# gzip -d /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz
```

Теперь мы готовы создать нашу базу данных zabbix. Снова станьте пользователем postgres и выполните следующую команду, чтобы создать базу данных как наш пользователь zabbix-srv:

```
# su - postgres
# createdb -E Unicode -O zabbix-srv  zabbix
```
Давайте проверим, действительно ли мы подключены к базе данных с правильной сессией.
Войдите из оболочки Postgres в базу данных zabbix

```
# psql -d zabbix -U zabbix-srv
```

Убедитесь, что вошли в систему под правильным пользователем ``zabbix-srv``.

```
zabbix=> SELECT session_user, current_user;
 session_user | current_user
--------------+--------------
 zabbix-srv   | zabbix-srv
(1 row)
```

PostgreSQL работает немного иначе, чем MySQL или MariaDB, когда дело касается почти всего :) Одна из вещей, которая есть у PostgreSQL и которой нет у MySQL - это, например, схемы. Если вы хотите узнать об этом больше, я могу порекомендовать [это URI](https://hevodata.com/learn/postgresql-schema/#schema). Там подробно объясняется, что это такое и зачем нам это нужно. Но вкратце...  В PostgreSQL схема обеспечивает многопользовательскую среду, позволющую нескольким пользователям получать доступ к одной и той же базе данных без помех. Схемы важны, когда несколько пользователей используют приложение и обращаются к базе данных по-своему или когда различные приложения используют одну и ту же базу данных. Существует стандартная схема, которую можно использовать, но лучше создать собственную схему.

???+ note
    "Существует стандартная схема ```public```, которую можно использовать, но лучше создать собственную схему, чтобы впоследствии, если рядом с базой данных Zabbix будет установлено что-то еще, было проще создавать пользователей с доступом только к таблицам вновь созданной базы данных."

```
zabbix=> CREATE SCHEMA zabbix_server AUTHORIZATION "zabbix-srv";
CREATE SCHEMA
zabbix=> set search_path to "zabbix_server";
zabbix=> \dn
          List of schemas
     Name      |       Owner
---------------+-------------------
 public        | pg_database_owner
 zabbix_server | zabbix-srv
(2 rows)


```
Теперь у нас готова БД с правильными правами для пользователя ```zabbix-srv```, но еще нет для нашего пользователя ```zabbix-web```. Давайте сначала исправим это и дадим права на подключение к нашей схеме.

```
zabbix=# GRANT USAGE ON SCHEMA zabbix_server TO "zabbix-web";
GRANT
```

Пользователь ```zabbix-web``` теперь имеет права на подключение к нашей схеме, но пока не может ничего сделать, давайте исправим это, но при этом не давайте слишком много прав.

```
zabbix=# GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA zabbix_server TO "zabbix-web";
GRANT
zabbix=# GRANT SELECT, UPDATE ON ALL SEQUENCES IN SCHEMA zabbix_server TO "zabbix-web";
GRANT
```

Вот так, оба пользователя созданы с правильными правами.
Теперь мы готовы заполнить базу данных структурами таблиц Zabbix и т.д. ... снова войдите в систему под пользователем postgres и выполните следующие команды 

Давайте загрузим SQL-файл Zabbix, который мы извлекли ранее, чтобы заполнить нашу базу данных необходимыми схемами, образами пользователей и т.д. ...

???+ warning
    "В зависимости от скорости вашего оборудования или виртуальной машины это может занять от нескольких секунд до нескольких минут, поэтому, пожалуйста, не отменяйте работу, просто сидите и ждите подсказки."


```
zabbix=# \i /usr/share/zabbix-sql-scripts/postgresql/server.sql
CREATE TABLE
CREATE INDEX
...
...
INSERT 0 1
INSERT 0 1
INSERT 0 1
INSERT 0 1
COMMIT
zabbix=#
```

???+ note
    "Если импорт завершился неудачей с сообщением ```psql:/usr/share/zabbix-sql-scripts/postgresql/server.sql:7: ERROR: no schema has been selected to create in```, то, вероятно, вы допустили ошибку в строке, где задали путь поиска."

Давайте проверим, правильно ли созданы наши таблицы с правильными разрешениями

```
zabbix=# \dt
                        List of relations
    Schema     |            Name            | Type  |   Owner
---------------+----------------------------+-------+------------
 zabbix_server | acknowledges               | table | zabbix-srv
 zabbix_server | actions                    | table | zabbix-srv
 zabbix_server | alerts                     | table | zabbix-srv
 zabbix_server | auditlog                   | table | zabbix-srv
 zabbix_server | autoreg_host               | table | zabbix-srv
...
...
 zabbix_server | usrgrp                     | table | zabbix-srv
 zabbix_server | valuemap                   | table | zabbix-srv
 zabbix_server | valuemap_mapping           | table | zabbix-srv
 zabbix_server | widget                     | table | zabbix-srv
 zabbix_server | widget_field               | table | zabbix-srv
(173 rows)
```

???+ note
    "Если вы похожи на меня и вам не нравится каждый раз при входе в систему с пользователем zabbix-srv устанавливать правильный путь поиска, то можете выполнить следующий запрос SQL. ```zabbix=> alter role «zabbix-srv» set search_path = «$user», public, zabbix_server;```"


Если вы готовы, то можете выйти из базы данных и вернуться под пользователем root.

```
zabbix=>  \q
# exit
```

### Настройка брандмауера

И последнее, что нам нужно сделать - это открыть брандмауэр и разрешить входящие соединения для базы данных PostgreSQL с нашего сервера Zabbix, потому что на данный момент мы пока не можем принимать никаких соединений.

```
# firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources:
  services: cockpit dhcpv6-client  ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

Сначала мы создадим соответствующую зону для нашей СУБД PostgreSQL и откроем порт 5432/tcp, но только для ip с нашего сервера Zabbix.

```
# firewall-cmd --new-zone=postgresql-access --permanent
success

# firewall-cmd --reload
success

# firewall-cmd --get-zones
block dmz drop external home internal nm-shared postgresql-access public trusted work

# firewall-cmd --zone=postgresql-access--add-source=<zabbix-serverip> --permanent

success
# firewall-cmd --zone=postgresql-access --add-port=5432/tcp  --permanent

success
# firewall-cmd --reload
```

Теперь давайте посмотрим на наши правила брандмауэра, чтобы убедиться, что они соответствуют нашим ожиданиям:

```
# firewall-cmd --zone=postgresql-access --list-all
```

```
postgresql-access (active)
  target: default
  icmp-block-inversion: no
  interfaces:
  sources: 192.168.56.18
  services:
  ports: 5432/tcp
  protocols:
  forward: no
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

Теперь наш сервер базы данных готов принимать соединения от нашего сервера Zabbix :).
Вы можете продолжить выполнение следующей задачи [Установка сервера Zabbix](installing-zabbix.ru.md)

