# Nginx Static File With Configuration on Docker by Example

## Tags

- Docker
- Nginx

## Aim

To set up a configuration file in nginx that hosts a static file from a configured location.

## Steps

For this we will use a compose file to keep the manual typing to a minimum.

### 1 Create two directories for configuration files and static files

``` bash
mkdir config
mkdir static
```

### 2 Create a config file

In the config directory create a file called `default.conf`
``` bash
cd config
touch default.conf
```
and fill it with the following content
``` nginx
server{
    listen 80;
    server_name nginx;
    root /static;
}
```

### 3 Create the static content

Move into the static directory and create a file called `index.htm`

``` bash
cd ../static
touch index.html
```
and add the following content to it
``` html
<!DOCTYPE html>
<html lang="en">

<head>
  <title>Hello World</title>
</head>

<body>
  <header>Hello World</header>
  <p>Hello from Nginx</p>
</body>

</html>
```

### 4 Docker Compose File

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
      - ./config:/etc/nginx/conf.d
      - ./static:/static
```

### 5 Bring The Service Up

Type the following into a shell that is in the same directory the `docker-compose.yml` files.
``` bash
docker compose up -d
```

### 6 Check Your Browser

Bring up your favourite browser and navigate to `localhost:8080` and see the example webpage from nginx.

### 7 Tidy Up

Once you are done, clean up by bringing your services down with the following shell command
``` bash
docker compose down
```

## Explanation

Here we are setting up an nginx configuration file for our container to use. Nginx looks in the `/etc/nginx/conf.d` directory for configuration files, in particular for a file called `default.conf`. We have created a bind mount to our configuration so that nginx looks in our host directory for it's config.

The config is set up to listen on port 80 and have a server name that is the same as it's dns name in docker (although this can be anything, as you can find out by changing it and bringing the service back up). Then we have set the root to look for static content in the `/static` directory instead of the default one.
