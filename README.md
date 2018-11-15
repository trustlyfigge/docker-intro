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
docker rm -f nginx
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
docker run --name shell --network www
wget webserver
```

Now we can connect to the server that we started :) 


## Extra:

Lets expand on nginx and create our own Docker file, with our own index.html.


Steps 
Select nginx version: 
[dockerhub nginx](https://hub.docker.com/_/nginx/)

```
FROM nginx:<tag>
```

Lets copy over our own index.html (which you have to create)

```
COPY <host path> <guest path>
```
See the [Dockerfile reference](https://docs.docker.com/engine/reference/builder/#copy)


Now we need to build the image

```
docker build -t mynginx .
```

This will read the docker file and create an image with the name mynginx

```
docker images #you should be able to find _mynginx_ in the list
```

What should we do to run this image in our network that we created earlier?


