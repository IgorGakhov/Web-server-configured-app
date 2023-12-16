# Web-server-configured-app

## Развертывание с помощью Docker Compose

```
$ docker compose up -d
✔ redis 23 layers [⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿]      0B/0B      Pulled
✔ apache 6 layers [⣿⣿⣿⣿⣿⣿]			    0B/0B      Pulled
...
[+] Running 6/6
✔ Network web-server-configured-app_default                   Created
✔ Container redis                                             Started
✔ Container flask-app                                         Started
✔ Container apache                                            Started                                                  
✔ Container nginx-proxy                                       Started
```

## Ожидаемый результат

В списке контейнеров должно быть указано четыре работающих контейнера и сопоставление портов, как показано ниже:

```
$ docker ps
CONTAINER ID   IMAGE                                   COMMAND                  CREATED         STATUS                            PORTS                              NAMES
be30bab685d5   web-server-configured-app-nginx-proxy   "/docker-entrypoint.…"   6 seconds ago   Up 5 seconds (health: starting)   0.0.0.0:80->80/tcp                 nginx-proxy
1efa398fb4a5   redislabs/redismod                      "redis-server --load…"   7 seconds ago   Up 5 seconds (healthy)            0.0.0.0:6379->6379/tcp             redis
447dce57d8b0   web-server-configured-app-flask-app     "gunicorn -w 3 -t 60…"   7 seconds ago   Up 5 seconds (health: starting)   5000/tcp, 0.0.0.0:8000->8000/tcp   flask-app
fb05a2925592   httpd:alpine                            "httpd-foreground"       7 seconds ago   Up 5 seconds                      80/tcp                             apache
```

После запуска приложения перейдите к `http://localhost:80` в веб-браузере или запустите:

```
$ curl localhost:80
Hello World!
```

Остановитесь и удалите контейнеры.

```
$ docker compose down
[+] Running 5/4
✔ Container nginx-proxy                      Removed
✔ Container apache                           Removed
✔ Container redis                            Removed
✔ Container flask-app                        Removed
✔ Network web-server-configured-app_default  Removed
```

Выполнив описанные выше шаги, вы получите реверсный прокси-сервер NGINX и серверную часть Flask приложения. Общий маршрут запросов будет выглядеть следующим образом:

`Client -> NGINX -> WSGI -> Flask`
