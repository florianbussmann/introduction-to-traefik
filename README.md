# introduction-to-traefik

Start [Traefik reverse proxy](https://traefik.io/traefik/):
```shell
docker-compose up -d reverse-proxy
```

Now that we have a Traefik instance up and running, we will deploy an [example service](https://github.com/traefik/whoami) called `whoami`. It's a simple web service that outputs information about the machine it is deployed on (IP address, host, and so on). We start two instances of this [example service](https://github.com/traefik/whoami) using automated load balancing:
```shell
docker-compose up -d --scale whoami=2
```

You can access the service throught Traefik, on the domain name [whoami.docker.localhost](http://whoami.docker.localhost):

```shell
$ curl --header 'Host:whoami.docker.localhost' 'http://localhost:80/'
Hostname: abafe2563b27
IP: 127.0.0.1
IP: 172.23.0.3
RemoteAddr: 172.23.0.2:49498
GET / HTTP/1.1
Host: whoami.docker.localhost
User-Agent: curl/7.64.1
Accept: */*
Accept-Encoding: gzip
X-Forwarded-For: 172.23.0.1
X-Forwarded-Host: whoami.docker.localhost
X-Forwarded-Port: 80
X-Forwarded-Proto: http
X-Forwarded-Server: 79815a933ac0
X-Real-Ip: 172.23.0.1
```

When accessing the service multiple times you will get responses from the two different instances that have been launched.