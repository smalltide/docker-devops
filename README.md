# docker-devops
Learn Docker and Devops from Udemy course

1. Docker技術入門與實戰
   https://www.udemy.com/docker-china

2. docker devops course from development to production
   https://www.udemy.com/the-docker-for-devops-course-from-development-to-production

![alt text](https://github.com/smalltide/docker-devops/blob/master/screenshot.gif "docker-devops")

1. docker
2. docker-machine
3. virtualbox
4. vagrant

install ubuntu 14.04 and docker
```
  > install virtualbox (https://www.virtualbox.org/wiki/Downloads)
  > install vagrant (https://www.vagrantup.com/downloads.html)
  > vagrant init ubuntu/trusty64
  > vagrant up
  > vagrant status
  > vagrant ssh default
  > install docker in ubuntu 14.04 vm (https://docs.docker.com/engine/installation/linux/ubuntu/#install-from-a-package)
  > sudo docker run hello-world
```
install docker on macOS
```
  > install docker (https://store.docker.com/editions/community/docker-ce-desktop-mac)
```
using docker-machine on macOS
```
  > docker-machine ls
  > docker-machine create demo
  > docker-machine ssh demo
  > docker-machine env demo
  > docker-machine stop demo
  > docker-machine rm demo
```
using docker command pull and delete images
```
  > docker pull ubuntu
  > docker images
  > docker pull ubuntu:16.04
  > docker image rm ubuntu:16.04
```
build docker image using Dockerfile and push to docker hub
```
  > cd flask-hello-world
  > docker build -t smalltides/python-flask-demo:1.0 .
  > docker login (when first push)
  > docker push smalltides/python-flask-demo:1.0
  > docker pull smalltides/python-flask-demo:1.0
```
new version docker command style
```
  > docker image ls
  > docker image pull
  > docker image rm
  > docker container ls
```
using docker command to manage container
```
  > docker image pull busybox
  > docker container create busybox
  > docker container ls (for running)
  > docker container ls -a (for all)
  > docker container start b7dc003fda3f (container id)
  > docker container rm b7dc003fda3f (container id)
  > docker container create busybox sh -c "while true; do sleep 3600; done"
  > docker container start 5863d34351c4 (container id)
  > docker container stop 5863d34351c4 (container id)
  > docker container create --name demo busybox sh -c "while true; do sleep 3600; done" (--name for named container)
  > docker container run -d --name demo busybox sh -c "while true; do sleep 3600; done" (-d for background running)
```