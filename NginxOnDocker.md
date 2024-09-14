# Nginx on Docker by Example

## Tags

- Docker
- Nginx

## Aim

To get a very basic Nginx container running on docker for future experiments.

## Steps

For this we will use a compose file to keep the manual typing to a minimum.

### 1 Example HTML File

Create an example html file, the following will probably be suitable.
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
save this as `example.html`

### 2 Docker Compose File

Write your `docker-compose.yml` file. Use the following text to configure the nginx image we will be using.
``` yml
services:
    nginx:
        image: nginx
        ports:
          - 8080:8080
        volumes:
          - ./example.html:/usr/share/nginx/html
```
> Notice that we are using the `example.html` we wrote earlier.

### 3 Bring The Service Up

Type the following into a shell that is in the same directory as your `example.html` and `docker-compose.yml` files.
``` bash
docker compose up -d
```

### 4 Check Your Browser

Bring up your favourite browser and navigate to `localhost:8080` and see your wonderful example webpage.

### 5 Tidy Up

Once you are done, clean up by bringing your services down with the following shell command
``` bash
docker compose down
```

## Explanation

In this "By Example" we created an html file that we wanted nginx to host for us. To do this we used the nginx docker image rather than host nginx on our own machine. Rather than manually type out commands on the command line to create a container using that image, we used a docker compose file that stored our configuration. The configuration contained a port mapping from port 8080 inside the container to the host port of 8080. It also contained a bind mount volume, which mounted our example.html file inside the container in a file in the `usr/share/nginx/html` directory. This is the directory that nginx uses to look for a default html file if one exists. We then checked on the browser if our configuration was correct and saw that nginx was hosting our example website. After which we tided up after ourselves by bringing our services back down again.

## Experiments

If you would like to play with this setup some more. Try changing the `example.html` file while the container is running (after doing `docker compose up`) and see how the bind mount lets you edit files on your host machine and those changes are reflected inside the container.