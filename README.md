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


Настройка удаленного подключения к postgresql (добавляем строчки в файл pghba.conf):

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


```bash
sudo systemctl restart postgresql
```