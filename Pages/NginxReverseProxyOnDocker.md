# Nginx Reverse Proxy on Docker

## Tags

- Docker
- Nginx

## Aim

To set up a configuration file in nginx that reverse proxies to two other services.

## Steps

For this we will use a compose file to keep the manual typing to a minimum.

### 1 Create a directory for configuration

``` bash
mkdir config
```

### 2 Create a config file

In the config directory create a file called `default.conf`
``` bash
cd config
touch default.conf
```
and fill it with the following content
``` nginx
server {
    listen 80;
    server_name nginx;

    location /a {
        proxy_pass http://backendA:3000/;
    }
    location /b {
        proxy_pass http://backendB:3000/;
    }
}
```

### 3 Docker Compose File

In the parent directory create the docker compose file
``` bash
cd ..
touch docker-compose.yml
```
Use the following content to configure the nginx image we will be using.
``` yml
services:
  nginx:
    image: nginx
    ports:
      - 8080:80
    volumes:
      - ./nginx-config:/etc/nginx/conf.d
  backendA:
    image: ghcr.io/hannahellis4242/ps/play-server:latest
  backendB:
    image: ghcr.io/hannahellis4242/ps/play-server:latest
```

> We will be using two additional services, backendA and backendB.

### 5 Bring The Service Up

Type the following into a shell that is in the same directory the `docker-compose.yml` files.
``` bash
docker compose up -d
```

### 6 Check Your Browser

Bring up your favourite browser and navigate to `localhost:8080/a` and see the output from serverA. Navigate to `localhost:8080/b` and see the output from serverB.

### 7 Tidy Up

Once you are done, clean up by bringing your services down with the following shell command
``` bash
docker compose down
```

## Explanation

Here we are setting up an nginx configuration file for our container to use. Nginx looks in the `/etc/nginx/conf.d` directory for configuration files, in particular for a file called `default.conf`. We have created a bind mount to our configuration so that nginx looks in our host directory for it's config.

The config is set up to listen on port 80 and have a server name that is the same as it's dns name in docker (although this can be anything, as you can find out by changing it and bringing the service back up). Then we have set two locations `/a` and `/b` that use the `proxy_pass` command to redirect all traffic to the urls included after the command.
