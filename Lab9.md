1. Используйте docker compose для резервного копирования volume статического контента произвольного сайта.
```
version: '3.8'

services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - nginx-data:/usr/share/nginx/html

volumes:
  nginx-data:
```

Запуск контейнера

```
docker-compose up -d
```

Замена содержимого

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Test Page</title>
</head>
<body>
    <h1>Что то измененное</h1>
</body>
</html>
```

Копируем файл в контейнер
```
docker cp ./index.html $(docker ps -q --filter "name=web"):/usr/share/nginx/html/
```
тоже самое, но  с томом 
```
docker run --rm --volumes-from $(docker ps -q --filter "name=web") -v $(pwd):/bubntu busybox tar cvf /bubuntu/bubuntu.tar /usr/share/nginx/html
```
2. Установите сервер времени(NTP) в контейнер на базе ubentu, сконфигурируйте два контейнера один на базе centos и другой на основе alpine Linux.
 Создаем докерфайл для нтп
 ```
 FROM ubuntu:latest

RUN apt-get update && apt-get install -y ntp && \
    echo "server 0.ubuntu.pool.ntp.org" >> /etc/ntp.conf && \
    service ntp start

CMD ["ntpd", "-g", "-n"]
```

Образ
``` 
docker build -t ntp-server .
```

Запуск контейнера 
```
 docker run --name ntp-server -d ntp-server 
```

и еще два контейнера на базе CentOS и Alpine
```
docker run -it --name centos-server centos:latest
docker run -it --name alpine-server alpine:latest
```
3. Напишите dockerfile и соберите образ для предыдущего задания.
```
# CentOS
FROM centos:latest as centos
RUN yum install -y ntp
```
```
# Alpine
FROM alpine:latest as alpine
RUN apk add --no-cache openntpd
```
5. Выполните резервное копирование и восстановление базы данных PostgreSQL или MongoDB
контейнер PostgreSQL:
запуск
```
docker run --name postgres-db -e POSTGRES_PASSWORD=mysecretpassword -d postgres
```
```
cat backup.sql | docker exec -i new-postgres-db psql -U postgres
```
```
docker run --name new-postgres-db -e POSTGRES_PASSWORD=mysecretpassword -d postgres
```