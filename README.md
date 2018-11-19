# Docker Demo

Let's start with just running a docker image:

```
docker run -d --name webserver -p 8080:80 nginx
```
This will start a docker image with nginx named webserver, with a mapping of the containers port 80 to the hosts port 8080.
Curl or visit localhost:8080

Run 

```
docker inspect webserver
```

Here you'll se a lot of information about the running docker image. 
Search for __NetworkSettings.Ports__

We can kill and remove nginx

```
docker rm -f webserver
```


Lets create a network and have some containers running in it:

```
docker network create www #creates the network
docker network ls #list available networks
```

Lets spin up a nginx server in there 
```
docker run -d --name webserver --network www nginx
```

Okay, let's spin up something else outside the network and see if we can access the webserver

```
docker run --name shell -it bash
wget webserver
```

Okay that didn't do much, let's see what happens if we run our container in the network we created.

```
docker rm -f shell # remove
docker run --name shell --network www --it bash
wget webserver
```

Now we can do a wget to the server that we started :) 


## Extra:

Lets expand on nginx and create our own Docker file, with our own index.html.


### Steps 

Select nginx version: 
[dockerhub nginx](https://hub.docker.com/_/nginx/)

```
FROM nginx:<tag>
```

Lets copy over our own index.html (which you'll have to create)
```
COPY <host path> /usr/share/nginx/html/index.html
```
See the [Dockerfile reference](https://docs.docker.com/engine/reference/builder/#copy)

Now we need to build the image

```
docker build -t mynginx .
```

This will read the docker file and create an image with the repository(name) mynginx and the tag latest

```
docker images #you should be able to find _mynginx_ in the list
```

What should we do to run this image in our network that we created earlier?


# Docker Compose

Docker compose is a tool for defining and running multiple containers.

The common workflow is to create a yaml file called docker-compose.yaml which contains your 
definition of containers. You can either use containers from docker-hub or another container registry.
In this tutorial we'll recreate the the steps above.

In the file docker-compose.yaml you can see two definitions, one for nginx and one for bash. The only
thing that differs is that the bash image has a sleep command. 
This is because docker needs a process to bind to, in this case sleep. For the nginx image it's the nginx 
daemon. 

Let's start with starting the images.
```
docker-compose up
```

Open another terminal window and run:
```
docker ps
```
here we can see the names of the running docker images.

To `log in` to a running container, we can execute the command:

```
docker exec -it <container name> bash
```














