# Описание
В данном репозитории находятся файлы для сборки образов [Docker](https://www.docker.com) с платформой [Система взаимодействия платформы 1С:Предприятие](https://v8.1c.ru/platforma/sistema-vzaimodeystviy/) 8.3.

# Использование
Пред использованием необходимо скачать и распаковать дсистрибутивы в три директории:
```bash
cs\1c_cs
elasticsearch\1c_cs
hazelcast\1c_cs
```
## Как запустить в docker-compose
:exclamation: Тестировалось только на версии 1c-cs-12.0.24-linux-x86_64

```bash
$ docker-compose up -d
```

## Как проверить в docker контейнере
:exclamation: Проверять внутри контейнера.

```bash
sudo curl http://localhost:8087/rs/health
```

## Инициализация базы данных 
:exclamation: Необходимо выполнить вручную. Выполнять внутри контейнера.

```bash
sudo curl -Sf -X POST -H "Content-Type: application/json" -d "{ \"url\" : \"jdbc:postgresql://db:5432/cs_db\", \"username\" : \"postgres\", \"password\" : \"postgres\", \"enabled\" : true }" -u admin:admin http://localhost:8087/admin/bucket_server
```