# Meowtopia
Site with the ability to publish photos of cats and their achievements.
![ezgif-4-48cbfaac1f](https://github.com/fabilya/bs4_parser_pep/assets/105780672/952addb9-56c8-433d-9733-a4b395155d4a)
## Table of content
- [Description](#description)
- [Technologies](#technologies-and-necessary-tools)
- [Start project](#how-to-start-a-project)
  - [Docker](#creating-docker-images)
  - [Deploy](#deploy-to-a-remote-server)
- [Author](#author)

### Description
<b>Meowtopia</b> — social network for sharing photos of your favorite pets. Allows you to post profiles of cats indicating the name, year of birth, color (to choose from a palette), and any achievements that the owner can come up with. The cat's profile can be with or without a photo. Only the owner who added it can edit a pet's profile. The project is deployed on the server in containers. Automatic testing of updates to the main branch and restarting the application on the server is carried out using the github Actions cloud service.

### Technologies and necessary tools

```
Docker
Postgres
Python 3.x
Node.js 9.x.x
Git
Nginx
Gunicorn
Django (backend)
React (frontend)
```

### How to start a project
* Clone the repository
```
git clone git@github.com:sukhartsev-s/kittygram_final.git
cd kittygram
```
* Create a .env file and fill it with your data:
```
POSTGRES_USER=[database_username]
POSTGRES_PASSWORD=[password_to_database]
POSTGRES_DB= [database_name]
DB_PORT=[port_connection_to_base]
DB_HOST=[db]
```
```
SECRET_KEY='SECRET_KEY'
DEBUG=False
ALLOWED_HOSTS='YOUR_DOMAIN'
```

### Creating Docker images
```
cd frontend
docker build -t fabilya/kittygram_frontend .
cd ../backend
docker build -t fabilya/kittygram_backend .
cd ../nginx
docker build -t fabilya/kittygram_gateway .
```
* Upload images to DockerHub:
```
docker push fabilya/kittygram_frontend
docker push fabilya/kittygram_backend
docker push fabilya/kittygram_gateway
```

### Deploy to a remote server
* Connect to a remote server

`ssh -i path_to_file_with_SSH_key/name_of_file_with_SSH_key user_name@server_ip_address`
* Create a kittygram directory on the server via the terminal

`mkdir kittygram`

* Installing docker compose on the server
```Bash
sudo apt update
sudo apt install curl
curl -fSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo apt-get install docker-compose-plugin
```

* Copy the docker-compose.production.yml and .env files to the kittygram/ directory:

```
scp -i path_to_SSH/SSH_name docker-compose.production.yml username@server_ip:/home/username/kittygram/docker-compose.production.yml
```

* Run docker compose in daemon mode

`sudo docker compose -f docker-compose.production.yml up -d`

* Perform migrations, collect backend statics and copy them to /backend_static/static/:
```
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
```

* On the server, in the nano editor, open the Nginx config:

`sudo nano /etc/nginx/sites-enabled/default`

* Add location settings in the server section
* 
``location / {
    proxy_set_header Host $http_host;
    proxy_pass http://127.0.0.1:9000;
}``

* Check the functionality of the configurations and restart Nginx:
```
sudo nginx -t
sudo service nginx reload
```

### Author
[Ilya Fabiyanskiy](http://github.com/fabilya)
