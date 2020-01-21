# docker cheatsheet

## general
- `docker --version`
	> _docker engine version_  
    > `Docker version 19.03.5, build 633a0ea`

- `docker version`
	> _docker version details_  

- `docker ps`  
	> _lists running containers_

- `docker ps -a`
	> _lists all containers_
	
- `docker events`
    > _get real time events from the server (when a container stops, an image is pushed, daemon reloads etc)_

- `docker system df`
    > _shows docker disk usage_


## quick start from hub.docker.com
`git clone https://github.com/docker/doodle.git`  
`cd doodle\cheers2019`  
`docker build -t erencelik/cheers2019 .`  
`docker run -it --rm erencelik/cheers2019`  
`docker login`  
`docker push erencelik/cheers2019`  

## images
- `docker images`
    >_lists all images_

- `docker tag erencelik/dummy-project:1.0.0.RELEASE erencelik/dummy-project:latest`
    >_docker tag SOURCE\_IMAGE\[:TAG\] TARGET\_IMAGE\[:TAG\]_  
    >_tags an image_

- `docker pull ubuntu`
    >_downloads the image_
    >_if no tag specified, downloads 'latest' as default_

- `docker image rm <iid>`
    >_removes image locally_
    >_first 2-3 chars of image id is enough as long as it's unique in local_

- `docker search ubuntu`
    >_searches images from docker hub and lists_

- `docker image history <iid>`
    >_shows the history of an image_
    >_if you get the error:_
    _unable to delete - image has dependent child images_
    _try using repository name and tag instead of image id:_
    `docker image rm ubuntu:latest`

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

- `docker container ru n hello-world` or `docker run hello-world`
    >_runs a container_
    >_if not available locally, downloads_

- `docker run -p 1903:8080 erencelik/dummy-project`
    >_publish exposed port(8080) of project to host(1903)_
    _host-port:container-port_

- `docker run -p 1903:8080 -d erencelik/dummy-project`
    >_runs in detached mode (background)_

- `docker run -it ubuntu`
    >_i for interactive and t for tty_
    >_it runs the container and takes you straight inside the container_  
    >_here it creates an interactive bash shell in ubuntu_
    >_'exit' command will exit shell_

- `docker run -dit ubuntu`
    >_we do the same but in detached mode_
    _it creates shell but we don't directly go there_
      - `docker exec -it <cid> sh`  
        >_with this command we can go shell after running in detached mode_

- `docker run -tid --name my-ubuntu ubuntu`
    >_assigns a name to the container_

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
    _for example you can run some commands in a running container via exec, make some changes and immediately create a new image with the latest situation_
    _takes two parameters: docker commit CONTAINER REPOSITORY[:TAG]_


## creating image manually

- `docker run -dit openjdk:8-jdk-alpine`

- `docker container exec <cid> ls /tmp`

- `docker images`

- `docker container ls`

- `docker container cp .\target\dummy-project.jar cid:/tmp`  
    >_copy_


- `docker container exec <c_id> ls /tmp`  
    > _we check if copy successful_


- `docker container commit --change='CMD ["java","-jar","/tmp/hello-world-rest-api.jar"]' practical_solomon in28min/hello-world-rest-api:manual1` 
    > _saves the container we creataed as an image_
    _in28min... -> repo name we want to give_
    _change: jar file will be launched at startup_

- `docker images`

- `docker run in28min/hello-world-rest-api:manual1`


## Dockerfile commands

- `FROM openjdk:8-jdk-alpine`  
    > _base image_  
    _a Dockerfile must start with FROM_  
    _FROM can appear multiple times within a single Dockerfile_  
    _a name can be given to a new build stage by adding AS name_  

- `ADD target/hello-world-rest-api.jar hello-world-rest-api.jar`
    > _ADD copies files or directories_  
    _ADD \<src> \<dest>_  
    _ADD and COPY are functionally similar, generally COPY is preferred_  
    _COPY only supports the basic copying of local files, while ADD has some extra features_  
- `ENTRYPOINT ["sh", "-c", "java - jar /hello-world-rest-api.jar"]`  
    > _ENTRYPOINT has two forms:_  
    _ENTRYPOINT ["executable", "param1", "param2"] (exec form, preferred)_  
    _ENTRYPOINT command param1 param2 (shell form)_  
    _configures a container that will run as an executable_  
    _sh means shell_  
    _\-c means commands are read from string_  
    
    ....devam edecek
    



## run a container from ubuntu image with a specific name
## install nginx in this container
## view nginx welcome page and exit 


- `docker run -tid --name my-ubuntu ubuntu`
- `docker exec -it my-ubuntu sh`

or just

- `docker run -ti --name my-ubuntu ubuntu`

- `apt install nginx`
    >_if you get this error: Unable to locate package nginx_
    >_run this command and try again:
    `_apt update`
    _in ubuntu, it's always suggested to run an update before installing someting new_

- `service nginx start`

- `apt install curl`

- `curl localhost`

- `exit`









created repo from hub.docker.com named first-repo




docker push erencelik/first-repo:0.0.1











------pipeline script---------

instead of git conf in project
and
build - execute shell:

docker build -t dummy-eren .
echo eren > deneme
docker run -tid -p 9090:8080 dummy-eren

pipeline: (Jenkinsfile)


pipeline {
    agent { label 'master' }
    stages {
        stage('git clone') {
            steps {
                sh 'rm -rf dummy-spring-boot; git clone https://gitlab.com/celikeren/dummy-spring-boot.git'
            }
        }
        stage('build') {
            steps {
                sh 'docker build -t erencelik/dummy-eren /var/lib/jenkins/workspace/eren'
            }
        }
        stage('run') {
            steps {
                sh 'docker run -tid -p 9090:8080 erencelik/dummy-eren'
            }
        }
        stage('push') {
            steps {
                sh 'docker push erencelik/dummy-eren'
            }
        }
    }
}


---------------------------------

dockerfile:
FROM maven:3.6.3-jdk-8 as builder
COPY . /app
WORKDIR /app
RUN mvn -Dmaven.test.skip=true package
FROM openjdk:8-alpine
WORKDIR /app
COPY --from=builder /app/target/*.jar ./app.jar
CMD ["java", "-jar", "app.jar"]


-------------------------------------------------------------------------------------




















## nginx index.html browser'dan görüntüle

echo "eren" > index.html
docker run -tid -p 5000:80 -v C:\Users\eren.celik:/usr/share/nginx/html nginx


FROM maven:3-jdk-8 
//projede testler olduğu için jdk. normalde mvn package için jdk'ya gerek yok.
COPY . /app
WORKDIR /app
RUN mvn package
CMD ["java", "-jar", ""]

docker build . -f Dockerfile









doğru:

FROM maven:3.6.3-jdk-8
COPY . /app
WORKDIR /app
RUN mvn -Dmaven.test.skip=true package
CMD ["java", "-jar", "target/helloworld-0.0.1-SNAPSHOT.jar"]

docker build -t workshop:0.0.1 .

docker run -ti -p 1903:8080 workshop:0.0.1







multi-stage:

FROM maven:3.6.3-jdk-8 as builder
COPY . /app
WORKDIR /app
RUN mvn -Dmaven.test.skip=true package
FROM openjdk:8-alpine
WORKDIR /app
COPY --from=builder /app/target/*.jar ./app.jar
CMD ["java", "-jar", "app.jar"]


docker build -t workshop:0.0.2 .

docker run -ti -p 1903:8080 workshop:0.0.2

















-------------------jenkins--------------------------------

ci (continous integration) tool
amacı entegrasyon
otomasyon aracı
projenin derlenmesi, testlerin çalışması, uygulamanın dağıtılmasını otomatize edebilmektedir

--gitlab ci (credentials yönetme zor, docker in docker) 
--travis (güzelliği debug var)


config.xml
workspace


pipeline / free style projects


credentials
    ssh key



docker pull jenkins/jenkins:2.214-centos

run -p 8080:8080 -p 50000:50000 jenkins/jenkins:2.214-centos

jenkins'in içine docker kurmak gerekiyor.
Dockerfile'ı çalıştırabilmesi için.








git add ***
git status
git commit -m ""
git push


