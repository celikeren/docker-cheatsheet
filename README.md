# docker tutorial notes

## general
- `docker --version`

- `docker events`
    > _get real time events from the server_


- `docker system df`
    > _general info about images, containers, cache etc_
  _shows docker disk usage_


## quick start
`git clone https://github.com/docker/doodle.git`  
`cd doodle\cheers2019`  
`docker build -t erencelik/cheers2019 .`  
`docker run -it --rm erencelik/cheers2019`  
`docker login`  
`docker push erencelik/cheers2019`  


## container
- `docker container run in28min/todo-rest-api-h2:1.0.0.RELEASE`
    >_does the same:_
    - `docker run in28min..`


- `docker run -p 1903:5000 in28min..`
    >_publish exposed port(5000) of project to host(1903)_
    _host-port:container-port_

- `docker run -p 1903:5000 -d in28min..`
    >_detached mode_

- `docker container pause <id>`
    >docker container unpause <id>

- `docker container inspect <id>`

- `docker container prune`
    >_removes all stopped containers_

- `docker container kill <id>`
    >_immediate stop_

- `docker run -p 5000:5000 --restart=always in28min..`
    >_when you restart docker desktoerep, the container restarts too._
    >_restart=always or restart=no. default is no._

- `docker top <c_id>`
    >_displays the running processes of container_

- `docker stats`
    >_shows all the stats about the running containers_

- `docker run -p 5001:5000 -m 512m in28min..`
    >_specific max memory: 512m / 1G_

- `docker run -p 5001:5000 --cpu-quota 5000 in28min..`
    >_typically entire quota which is present is 100.000_
    >_how much cpu quota you want to allocate (%5 here)_

- `docker run -dit openjdk:8-jdk-alpine`
    >_dit: detached and interactive shell_


## images
- `docker images`
    >_shows images_

- `docker tag in28min/todo-rest-api-h2:1.0.0.RELEASE in28min/todo-rest-api-h2:latest`

- `docker pull mysql`

- `docker image remove <id>`
    >_removes locally_

- `docker search mysql`

- `docker image history <id>`

- `docker image inspect <id>`


## logs
- `docker logs <id>`  
    >_first 4 chars of id_

- `docker logs -f <id>`  
    >_follow/tail logs_

- `docker container ls`  
    >_shows the running containers_

- `docker container ls -a`  
    >_all -> terminated containers included_

- `docker container stop <id>`  


## creating image manually

- `cd ../01-hello-world-rest-api`

- `docker run -dit openjdk:8-jdk-alpine`

- `docker container exec <c_id> ls /tmp`

- `docker images`

- `docker container ls`

- `docker container cp .\target\hello-world-rest-api.jar practical_solomon:/tmp`  
    >_copy_
    _use c_id instead of nickname_


- `docker container exec <c_id> ls /tmp`  
    > _we check if copy successful_


- `docker container commit --change='CMD ["java","-jar","/tmp/hello-world-rest-api.jar"]' practical_solomon in28min/hello-world-rest-api:manual1` 
    > _saves the container we creataed as an image_
    _in28min... -> repo name we want to give_
    _change: jar file will be launched at startup_

- `docker images`

- `docker run in28min/hello-world-rest-api:manual1`


## from Dockerfile

- `FROM openjdk:8-jdk-alpine`  
    > _base image_  
    _a Dockerfile must start with FROM_  
    _FROM can appear multiple times within a single Dockerfile_  
    _a name can be given to a new build stage by adding AS name_  

- `ADD target/hello-world-rest-api.jar hello-world-rest-api.jar`
    > _ADD copies files or directories_  
    _ADD \<src> \<dest>  
    _ADD and COPY are functionally similar, generally COPY is preferred_  
    _COPY only supports the basic copying of local files, while ADD has some extra features_  
- `ENTRYPOINT ["sh", "-c", "java - jar /hello-world-rest-api.jar"]`  
    > _ENTRYPOINT has two forms:_  
    _ENTRYPOINT ["executable", "param1", "param2"] (exec form, preferred)_  
    _ENTRYPOINT command param1 param2 (shell form)_  
    _configures a container that will run as an executable_  
    _sh means shell_  
    _\-c means commands are read from string_  
    
    
    
#### docker training

- health check?

- docker version

- docker ps  
_running containers_

- docker ps -a

- docker run
_bir imajdan container ayağa kaldırır_

- docker build
_verilen dockerfile'dan yeni bir image oluşturur_

- docker cp
_copies file_

- docker exec
_verilen komutu çalştırır_

- docker logs
_container loglarını gösterir_

- docker start
_id'si verilen container'ı çalştırır_

- docker stop
_stops the container_



- docker pull hello-world

- docker ps
_gözükmüyor_

- docker ps -a

_işini yaptı kendini kapattı_


## run a container from ubuntu image with a specific name
## install nginx in this container
## view nginx welcome page 

docker run -tid --name bjk-ubuntu ubuntu

docker exec -it bjk-ubuntu sh

apt install nginx

_apt update_

service nginx start

apt install curl

curl localhost

exit







docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname






created repo from hub.docker.com named first-repo

docker commit bjk-ubuntu erencelik/first-repo:0.0.1

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










































