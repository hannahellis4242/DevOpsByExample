# Nginx With Configuration on Docker by Example

## TODO

handy links
- [docker block on how to setup nginx within docker](https://www.docker.com/blog/how-to-use-the-official-nginx-docker-image/)

## Tags

- Docker
- Nginx

## Aim

To get a very basic Nginx container running on docker with a configuration for future experiments.

## Steps

For this we will use a compose file to keep the manual typing to a minimum.

### 1 Create a directory for configuration files to live in

``` bash
mkdir nginx-local
```

### 2 Create a config file

``` nginx
events {}

http {
    server {
        listen 80;
        server_name 
    }
}
```

### 1 Docker Compose File

Write your `docker-compose.yml` file. Use the following text to configure the nginx image we will be using.
``` yml
services:
    nginx:
        image: nginx
        ports:
          - 8080:80
```

### 3 Bring The Service Up

Type the following into a shell that is in the same directory as your `example.html` and `docker-compose.yml` files.
``` bash
docker compose up -d
```

### 4 Check Your Browser

Bring up your favourite browser and navigate to `localhost:8080` and see the example webpage from nginx.

### 5 Tidy Up

Once you are done, clean up by bringing your services down with the following shell command
``` bash
docker compose down
```

## Explanation

Here we were just getting a nginx container running so that we could see it. To do this we used the nginx docker image rather than host nginx on our own machine. Also instead of manually typing out commands on the command line to create a container using that image, we used a docker compose file that stored our configuration. The configuration contained a port mapping from port 80 inside the container to the host port of 8080. After which we tided up after ourselves by bringing our services back down again.
