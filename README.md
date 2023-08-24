# Kittygram - блог для размещение фотографий котиков.
## Описание проекта:
Проект Kittygram даёт возможность пользователям поделиться и похвастаться фотографиями своих любимымих котиков. Зарегистрированные пользователи могут создавать, просматривать, редактировать и удалять свои записи.

## Установка проекта:
 * Клонироуйте репозиторий:

`git clone git@github.com:sukhartsev-s/kittygram_final.git`

`cd kittygram`

* Создайте файл .env и заполните его своими данными:

# Секреты DB
`POSTGRES_USER=[имя_пользователя_базы]`

`POSTGRES_PASSWORD=[пароль_к_базе]`

`POSTGRES_DB= [имя_базы_данных]`

`DB_PORT=[порт_соединения_к_базе]`

`DB_HOST=[db]`

# Секреты джанги
`SECRET_KEY='SECRET_KEY'`

`DEBUG=False`

`ALLOWED_HOSTS='ваш домен'`

# Создание Docker-образов
## Замените username на ваш логин на DockerHub:

`cd frontend`

`docker build -t username/kittygram_frontend .`

`cd ../backend`

`docker build -t username/kittygram_backend .`

`cd ../nginx`

`docker build -t username/kittygram_gateway . `

# Загрузите образы на DockerHub:

`docker push username/kittygram_frontend`

`docker push username/kittygram_backend`

`docker push username/kittygram_gateway`

# Деплой на удалённый сервере
## Подключитесь к удаленному серверу

`ssh -i путь_до_файла_с_SSH_ключом/название_файла_с_SSH_ключом имя_пользователя@ip_адрес_сервера `

## Создайте на сервере директорию kittygram через терминал

`mkdir kittygram`

Установка docker compose на сервер:

`sudo apt update`

`sudo apt install curl`

`curl -fSL https://get.docker.com -o get-docker.sh`

`sudo sh ./get-docker.sh`

`sudo apt-get install docker-compose-plugin`

## В директорию kittygram/ скопируйте файлы docker-compose.production.yml и .env:

`scp -i path_to_SSH/SSH_name docker-compose.production.yml username@server_ip:/home/username/kittygram/docker-compose.production.yml`

## Запустите docker compose в режиме демона:

`sudo docker compose -f docker-compose.production.yml up -d`

## Выполните миграции, соберите статику бэкенда и скопируйте их в /backend_static/static/:

`sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate`

`sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic`

`sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/`

## На сервере в редакторе nano откройте конфиг Nginx:

`sudo nano /etc/nginx/sites-enabled/default`

## Добавте настройки location в секции server:

``location / {
    proxy_set_header Host $http_host;
    proxy_pass http://127.0.0.1:9000;
}``

# Проверьте работоспособность конфигураций и перезапустите Nginx:

`sudo nginx -t `

`sudo service nginx reload`

Cсылка на развёрнутое приложение:

http://kittygraamm.bounceme.net/

## Технологии и необходимые инструменты:

Docker

Postgres

Python 3.x

Node.js 9.x.x

Git

Nginx

Gunicorn

Django (backend)

React (frontend)

# Автор

## Фабиянский Илья
