# Installing Docker

## as standlone in PC
<https://docker.com/products/docker-desktop>

## in aws instance

<https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#instance:v3>\
* search ec2
    * ec2 console
* Launch instance button
    * Select 64-bit(x86) 
    * Next: Configure Instace Details
    * click next with all defalt option
        * once create select from list
        * copy open IPv4 addess (open address) from instance Details
* MobaXtterm (<https://mobaxterm.mobatek.net/download.html>)
    ### to connect ec2 instance
    * Open new Tab from Terminal Menu option
        * select SSH tab
            * Remote Host paste IPv4 address (copied from ec2 instance open address)

## Red hat Linux
### package manager is yum
update os

```
sudo su
yum update -y
```
### install dockcer with -y so it not prompt question and take all default option
```
yum install docker -y
```

#### docker status check

```
systemctl status docker
```

start the docker

```
systemctl start docker
systemctl status docker
docker info
```

# docker operation

## create docker image
<https://hub.docker.com> docker image repo

to start new container

`docker container run -it -d <image name>`
```
docker container run -it -d nginx
```
list out running container
```
docker container ls
```

`with all container active and inactive`
```
docker container ls -a
```
stop the container

`docker container stop <containerID full or start with>`

```
docker container stop 88b6418282ead5
```

start existing container

`docker container start <containerID full or start with>`
```
docker container start 88b
```

re-start existing container

`docker container restart <containerID full or start with>`
```
docker container restart 88b
```

`remove all container from docker`
```
docker rm -f $(docker ps -a -q)
```


# Container port mapping 
to access private IPaddress service to expose to access outside of host machine


88b > containerID
```
docker container inspect 88b
```
`docker container run -p <host_port>:<container_port> -itd nginx`

host machine port we want to open as 8080 and as nginx running on default 80 port so here we want container port as 80
```
docker container run -p 8080:80 -itd nginx
```
`now we can access from browser with container url:8080 to access the service or container application` 

\
\


container logs
`docker container logs <containerID full or start with>`

```
docker container logs f35
```

container naming

`docker container run --name <name of container> -p 90:80 -itd nginx`

```
docker container run --name mynginx -p 90:80 -itd nginx
```

execute commands inside container



`docker container exec <containerID full or start with> <command to execute>

```
docker container exec f35 ls -lrt
docker container exec f35 apt update -y
```
apt command is ubonto based package manager


\
\
\






## go inside the container to makedo some operations
bash or sh or ash is depend on OS like for ubunto we can use bash

`docker container exec -it <containerID full or start with> bash/sh/ash`

```
docker container exec -it dd bash
ls
env
apt update -y
apt install vim -y
```

`come out of container without stop it`
```
ctrl+ pq
```
## to check all process running in container instance

`docker container top <containerID full or start with>`
```
docker container top dd
```

## to see all containers memory uses or stats

```
docker container stats
```
`docker container run -itd ubuntu:14.0.2 if we want any spacific version`

```
docker container run -itd ubuntu
docker container exec -it 1d bash
ls tmp/
ctrl +pq
touch a.txt
docker container cp a.txt 1d:/tmp
docker container exec 1d ls tmp
docker container stop 1d
docker container rm 1d
docker container diff 1d
```
# `To remove all container`

```
docker container rm -f ${docker container ls -aq}
```


# Docker file

## some vi commands
```
vi <filename>
vi Dockerfile

To Exit:
:x => quit vi
:wq => quit vi and save
:q => quit vi
:q! => quit without save

i => to inset text

Esc
:wq

```



```
vi Dockerfile
```
`<instruction> <commands>`
```
FORM ubuntu
```

build the image from docker file\
`docker image build -t <image tagname> <Dockerfile location>`
```
docker image build -t myapp .

docker images
docker image ls
```

```
vi Dockerfile
FORM ubuntu
LABEL env="prod"

save and quit
docker image build -t myapp .
```
### Image inspect
`docker image inspect <image name>`
```
docker image inspect myapp
```


```
vi Dockerfile
FORM ubuntu
LABEL env="prod"
RUN apt update -y && apt install git -y
ENV user admin
ENV pass secret
WORKDIR /app
#COPY a.txt . # source <distination>
#ADD demo . #directoryname <distination>

# create tar file
tar cvf demo.tar demo/
ls
# ADD will copy and extracted also
# ADD also used to download file from network location and placed in container folder
ADD demo.tar . 


# switch user from root

RUN useradd test #create the user
USER test # switch to test user

# note:: CMD and ENTRYPOINT command line will run when container start, not on container build time

#CMD ["sh"] #overwrite cmd command with console command
ENTRYPOINT ["sh"] # it will append the command with input command like docker container run -itd myapp ping google.com




save and quit
docker image build -t myapp .
```

## Build Docker file for nginx Server with custom index file

```
vi Dockerfile

FROM amazonlinux
RUN yum update -y && amazon-linux-extras install nginx1 -y
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
Esc
:wq or :x
```
```

docker image build -t mynginx .

docker container run -p 8080:80 -itd mynginx
```
`let custmize index.html`

```
vi index.html

<h1>Hello From Nginx!</h1>

Esc
:x
```
```
vi Dockerfile

FROM amazonlinux
RUN yum update -y && amazon-linux-extras install nginx1 -y
COPY index.html /usr/share/nginx/html/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
Esc
:wq or :x
```



# Need to remove below line
we are in `Using Automation Code Developed and Pushed in CI/CD Session 1`

# Docker Network

```
docker network ls
```

## `create network`
`docker network create <networkName> -d <network driver bridge(default)/overlay>`

```
docker network ls
docker network create test -d bridge
```

### `network inspect`
`docker network inspect <network name>`\
`docker container inspect <container name>`\

```
docker network inspect test

docker container run --name custom1 --network test -itd ubuntu
docker container run --name custom2 --network test -itd ubuntu
docker network inspect test
docker container ls
docker container inspect 24|grep IPAdd
docker container exec -it 24 bash
apt update -y
apt install -y iputils-ping
```



`kube.docx`

# Kubernates

## minikube instalation
`minikube https://minikube.sigs.k8s.io/docs/start/`

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

minikube start --driver=docker # from driver page
```

## kuberneties (cli kubectl) Installation
 `https://kubernetes.io/docs/tasks/tools/`
 ```
 curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

 sudo chmod +x kubectl

 sudo mv kubectl /usr/bin/    # to add in path
 ```

```
kubectl get node
kubectl get pod

ls -lart # to see the hidden directry

cd .kube
ls
cat config

# .kube folder location in mac
/Users/<USERNAME>/.kube/config
```

![image text](k8b_arch.png "Arch")
