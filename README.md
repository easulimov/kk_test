# Настройки хостов

## main01 (postgresql14, nginx)


Установка postgresql
```bash
sudo apt update && sudo apt upgrade -y
```

```bash
sudo apt install postgresql
```

#### Создание баз даннных и пользователей


Подключиться к базе данных по умолчанию с пользователем postgres

```bash
sudo -u postgres psql template1
```

Установить пароль для пользователя postgres
```
ALTER USER postgres WITH ENCRYPTED PASSWORD 'ваш_пароль';
```


```
CREATE USER keycloak WITH PASSWORD '123';
```

```
CREATE DATABASE keycloak;
GRANT ALL PRIVILEGES ON DATABASE keycloak TO keycloak;
```

```
CREATE USER grafana WITH PASSWORD '123';
```

```
CREATE DATABASE grafana;
GRANT ALL PRIVILEGES ON DATABASE grafana TO grafana;
```


#### Настройка удаленного подключения к postgresql (добавляем строчки в файл /etc/postgresql/14/main/pg_hba.conf):

```
# Разрешение подключения пользователю keycloak к базе данных keycloak с любого IP по паролю
host keycloak keycloak 0.0.0.0/0 md5

# Разрешение подключения пользователю grafana к базе данных grafana с любого IP по паролю
host grafana grafana 0.0.0.0/0 md5
```
>`host` указывает на метод аутентификации

>`keycloak` и `grafana` - это имена пользователей

>`0.0.0.0/0` - разрешает подключение с любого IP

> `md5` - это метод хеширования пароля


Разрешение на прослушивание запросов со всех сетей ( /etc/postgresql/14/main/postgresql.conf)

```bash
sudo vim /etc/postgresql/14/main/postgresql.conf
```

Заменить `listen_addresses = 'localhost'` на `listen_addresses = '*'`


```bash
sudo systemctl restart postgresql@14-main.service
```


## grafana01

#### Установка и первоначальная настройка grafana

```bash
sudo apt-get install -y adduser libfontconfig1 musl
```

```bash
wget https://dl.grafana.com/oss/release/grafana_11.1.0_amd64.deb
```

```bash
sudo dpkg -i grafana_11.1.0_amd64.deb
```

```bash
sudo /bin/systemctl daemon-reload
```

```bash
sudo /bin/systemctl enable grafana-server
```

```bash
sudo /bin/systemctl start grafana-server
```


Подключение grafana к postgresql

```bash
sudo vim /etc/grafana/grafana.ini
```

Найти секцию [database] и отредактировать:

```ini
[database]
# You can configure the database connection by specifying type, host, name, user and password
# as separate properties or as on string using the url properties.

# Either "mysql", "postgres" or "sqlite3", it's your choice
;type = sqlite3
;host = 127.0.0.1:3306
;name = grafana
;user = root

type = postgres
host = 192.168.122.116:5432
name = grafana
user = grafana

# If the password contains # or ; you have to wrap it with triple quotes. Ex """#password;"""
;password =

password = 123
```


```bash
sudo /bin/systemctl restart grafana-server
```

```bash
systemctl status grafana-server
```

Проверка удаленного подключения с помощью psql

```bash
sudo apt install postgresql-client-14
```

```bash
psql -h 192.168.122.116 -U grafana -d grafana
```
