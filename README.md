# docker tutorial notes

## general
`docker --version`

`docker events`  
_get real time events from the server_


`docker system df`  
_general info about images, containers, cache etc_  
_shows docker disk usage_


## quick start
`git clone https://github.com/docker/doodle.git`  
`cd doodle\cheers2019`  
`docker build -t erencelik/cheers2019 .`  
`docker run -it --rm erencelik/cheers2019`  
`docker login`  
`docker push erencelik/cheers2019`  


## container
`docker container run in28min/todo-rest-api-h2:1.0.0.RELEASE`  
 _does the same:_
`docker run in28min..`

`docker run -p 1903:5000 in28min..`  
_publish exposed port(5000) of project to host(1903)_  
_host-port:container-port_  

`docker run -p 1903:5000 -d in28min..`  
_detached mode_

`docker container pause <id>`  
docker container unpause <id>

`docker container inspect <id>`

`docker container prune`  
_removes all stopped containers_

`docker container kill <id>`  
_immediate stop_

`docker run -p 5000:5000 --restart=always in28min..`  
_when you restart docker desktop, the container restarts too._
_restart=always or restart=no. default is no._

`docker top <c_id>`  
_displays the running processes of container_

`docker stats`  
_shows all the stats about the running containers_

`docker run -p 5001:5000 -m 512m in28min..`  
_specific max memory: 512m / 1G_

`docker run -p 5001:5000 --cpu-quota 5000 in28min..`  
_typically entire quota which is present is 100.000_
_how much cpu quota you want to allocate (%5 here)_

`docker run -dit openjdk:8-jdk-alpine`  
_dit: detached and interactive shell_


## images
`docker images`  
_shows images_

`docker tag in28min/todo-rest-api-h2:1.0.0.RELEASE in28min/todo-rest-api-h2:latest`

`docker pull mysql`

`docker image remove <id>`  
_removes locally_

`docker search mysql`

`docker image history <id>`

`docker image inspect <id>`  


## logs
`docker logs <id>`  
_first 4 chars of id_

`docker logs -f <id>`  
_follow/tail logs_

`docker container ls`  
_shows the running containers_

`docker container ls -a`  
_all -> terminated containers included_

`docker container stop <id>`  


## creating image manually

`cd ../01-hello-world-rest-api`

`docker run -dit openjdk:8-jdk-alpine`

`docker container exec <c_id> ls /tmp`

`docker images`

`docker container ls`

`docker container cp .\target\hello-world-rest-api.jar practical_solomon:/tmp`  
_copy_
_use c_id instead of nickname_


`docker container exec <c_id> ls /tmp`  
_we check if copy successful_


`docker container commit --change='CMD ["java","-jar","/tmp/hello-world-rest-api.jar"]' practical_solomon in28min/hello-world-rest-api:manual1` 
_saves the container we creataed as an image_
_in28min... -> repo name we want to give_
_change: jar file will be launched at startup_

`docker images`

`docker run in28min/hello-world-rest-api:manual1`


## from Dockerfile

`FROM openjdk:8-jdk-alpine`  
_base image_
_a Dockerfile must start with FROM_
_FROM can appear multiple times within a single Dockerfile_
_a name can be given to a new build stage by adding AS name_

`ADD target/hello-world-rest-api.jar hello-world-rest-api.jar`  
`ENTRYPOINT ["sh", "-c", "java - jar /hello-world-rest-api.jar"]`  



