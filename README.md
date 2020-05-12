# docker cheatsheet :whale:

## intro
- `docker --version`  
    > _docker engine version_  

- `docker version`
    > _docker version in detail_  
	
- `docker events`
    > _get real time events from the server_  
    _when a container stops, an image is pushed, daemon reloads etc_

- `docker system df`
    > _shows docker disk usage_


## quick start from hub.docker.com
`git clone https://github.com/docker/doodle.git`  
`cd doodle\cheers2019`  
`docker build -t erencelik/cheers2019 .`  
`docker run -it --rm erencelik/cheers2019`  
`docker login`  
`docker push erencelik/cheers2019`  

> _gets the repo, builds an image from the Dockerfile_  
_runs it as a container and pushes the image to your registry in hub.docker.com_

## images
- `docker images`
    >_lists all images_

- `docker tag erencelik/dummy-project:1.0.0.RELEASE erencelik/dummy-project:latest`
    >_tags an image_  
    >_docker tag SOURCE\_IMAGE\[:TAG\] TARGET\_IMAGE\[:TAG\]_

- `docker pull ubuntu`
    >_downloads the image_  
    >_if no tag specified, downloads 'latest' as default_

- `docker image rm <iid>`
    >_removes image locally_  
    >_first 2-3 chars of image id is enough as long as it's unique in local_  
    >_if you get the error:_  
    _unable to delete - image has dependent child images_  
    _try using repository name and tag instead of image id:_  
        `docker image rm ubuntu:latest`

- `docker search ubuntu`
    >_searches images from docker hub and lists_

- `docker image history <iid>`
    >_shows the history of an image_  

- `docker image inspect <iid>`
    >_shows detailed information of an image_

- `docker build .`
    >_builds an image from a Dockerfile in current directory_  
    _an url or path can be given_

- `docker push dummy-project:latest`
    >_push an image or a repository to a registry_



## containers

- `docker container ls`  
    >_shows the running containers_

- `docker container ls -a`  
    >_shows all containers_

- `docker ps`  
    >_shows the running containers_

- `docker ps -a`  
    >_shows all containers_

- `docker container run hello-world`  or  `docker run hello-world`
    >_runs a container_  
    >_if not available locally, downloads_

- `docker run -p 1903:8080 erencelik/dummy-project`
    >_publishes exposed port(8080) of project to host(1903)_  
    _host-port:container-port_

- `docker run -p 1903:8080 -d erencelik/dummy-project`
    >_runs in detached mode (background)_
    
- `docker run -p 8032:8032 -e "SPRING_PROFILES_ACTIVE=test" <iid>`
    >_run with environment_

- `docker run -it ubuntu`
    >_i for interactive and t for tty_  
    >_it runs the container and takes you straight inside the container_   
    >_here it creates an interactive bash shell in ubuntu_  
    >_'exit' command will exit shell_

- `docker run -dit ubuntu`
    >_we do the same but in detached mode_  
    _it creates shell but we don't directly go there_  
        `docker exec -it <cid> sh`  
        _with this command we can go shell after running in detached mode_

- `docker run -tid --name my-ubuntu ubuntu`
    >_assigns a name to the container_
    
- `docker run -it --rm erencelik/cheers2019`  
    >_--rm automatically removes the container when it exits_
    
- `docker container stop <cid>`   
    >_stops a running container_     

- `docker container pause <cid>`

- `docker container unpause <cid>`

- `docker container inspect <cid>`

- `docker container prune`
    >_removes all stopped containers_

- `docker container kill <cid>`
    >_immediate stop_

  
  
- `docker run -p 8080:8080 --restart=always erencelik/dummy-project`
    >_when you restart docker desktop, the container restarts too._  
    >_restart=always or restart=no. default is no._

- `docker top <cid>`
    >_displays the running processes of container_

- `docker stats`
    >_shows all the stats about the running containers_

- `docker run -p 8080:8080 -m 512m erencelik/dummy-project`
    >_specific max memory: 512m/1g_

- `docker logs <cid>`  
    >_shows the logs of a container_

- `docker logs -f <cid>`  
    >_follow logs_

- `docker cp `  
    >_copies files/folders between a container and the local filesystem_  
    _docker cp CONTAINER:SRC\_PATH DEST\_PATH_

- `docker commit my-ubuntu erencelik/dummy-project:0.0.2`
    >_creates a new image from a container’s changes_  
    _this command allows us to take a running container and save its current state as an image_  
    _for example you can run some commands in a running container via exec, make some changes_  
    _and immediately create a new image with the latest situation_  
    _takes two parameters: docker commit CONTAINER REPOSITORY[:TAG]_




### task:
1. run a container from ubuntu image with a specific name
2. install nginx in this container
3. view nginx welcome page and exit 


- `docker run -it --name my-ubuntu ubuntu`

- `apt install nginx`
    >_if you get this error: Unable to locate package nginx_  
    >_run this command and try again:  
    `apt update`  
    _in ubuntu, it's always suggested to run an update before installing someting new_

- `service nginx start`

- `apt install curl`

- `curl localhost`

- `exit`

