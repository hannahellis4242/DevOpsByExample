# Nginx on Docker by Example

## Tags

- Docker
- Nginx

## Aim

To host a static html file on Nginx. This is using a docker container to run nginx.

## Steps

### 1 Setting up a bind mount location

In create a new directory by using the following command in your bash command line.

``` bash
mkdir nginx-local
```

### 2 Creating a static html to host

Change into the directory you just made and create a new file called `index.html`

``` bash
cd nginx-local
touch index.html
```

Then put the following content into the `index.html` file

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

### 3 Docker Compose File

Change back to the parent directory and create a `docker-compose.yml` file
``` bash
cd ..
touch docker-compose.yml
```
with the following content
``` yml
services:
    nginx:
        image: nginx
        ports:
          - 8080:80
        volumes:
          - ./nginx-local:/usr/share/nginx/html
```

### 4 Bring The Service Up

To create the container just use the following command

``` bash
docker compose up -d
```

### 5 Check Your Browser

Bring up your favourite browser and navigate to `localhost:8080` and see the example webpage from nginx.

### 6 Tidy Up

Once you are done, clean up by bringing your services down with the following shell command
``` bash
docker compose down
```

## Explanation

Nginx by default looks for static html files in `/usr/share/nginx/html`. By bind mounting our local directory into this directory, the container can see our `index.html` and host it. When you visit `localhost:8080` nginx responds with the contents of `index.html` as that is considered the default html file.
