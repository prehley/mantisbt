`MantisBT` is an open source issue tracker that provides
a delicate balance between simplicity and power.

## docker-compose.yml

```
version: '2'
services:
  mantisbt:
    image: prehley/mantisbt:latest
    ports:
    - 8081:80/tcp
    links:
    - mysql:mysql
    restart: unless-stopped

  mysql:
    image: mysql:latest
    environment:
      MYSQL_PASSWORD: dbadminPassword
      MYSQL_ROOT_PASSWORD: rootPassword
      MYSQL_USER: dbadmin
    restart: unless-stopped
    volumes:
    - /sqldump:/docker-entrypoint-initdb.d
    command:
    - --default_authentication_plugin=mysql_native_password
```

> You can use `mariadb`/`postgres` instead of `mysql`.

## install

```
$ firefox http://localhost:8081/admin/install.php
>>> username: administrator
>>> password: root
```

```
==================================================================================
Installation Options
==================================================================================
Type of Database                                        MySQL/MySQLi
Hostname (for Database Server)                          mysql
Username (for Database)                                 mantisbt
Password (for Database)                                 mantisbt
Database name (for Database)                            bugtracker
Admin Username (to create Database if required)         root
Admin Password (to create Database if required)         root
Print SQL Queries instead of Writing to the Database    [ ]
Attempt Installation                                    [Install/Upgrade Database]
==================================================================================
```

To install a new installation leave the /sqldump directory empty.
To install from an existing mantisbt first export that database and then put the dump.sql file into the /sqldump directory

## email 

Append following to `/var/www/html/config_inc.php`

```
$g_phpMailer_method = PHPMAILER_METHOD_SMTP;
$g_administrator_email = 'root@example.org';
$g_webmaster_email = 'webmaster@example.org';
$g_return_path_email = 'mantisbt@example.org';
$g_from_email = 'mantisbt@example.org';
$g_smtp_host = 'smtp.example.org';
$g_smtp_port = 25;
$g_smtp_connection_mode = 'tls';
$g_smtp_username = 'mantisbt';
$g_smtp_password = '********';
```
