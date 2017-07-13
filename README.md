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
add now user to docker group
```
  > sudo groupadd docker
  > sudo gpasswd -a ${USER} docker
  > sudo service docker restart
  > exit
  > vagrant ssh default
```
manage multi image and container command
```
  > docker container ls -a
  > docker container ls -aq
  > docker container ls -f status=exited | awk '{print$1}' | awk 'NR>1' | xargs docker container rm (filter status=exited)
  > docker container start $(docker container ls -q -f status=created) (start all created status containers)
  > docker image ls -q
  > docker image rm $(docker image ls -q)
```
manage linux network name space and create peer pair
```
  > sudo ip netns list
  > sudo ip netns add blue
  > sudo ip netns delete blue
  > sudo ip netns exec blue ip a
  > sudo ip netns exec blue ip link set dev lo up (up loopback)
  > sudo ip link add blue-veth-a type veth peer name blue-veth-b
  > sudo ip link set blue-veth-b netns blue
  > sudo ip netns exec blue ip link
  > sudo ip addr add 192.168.1.1/24 dev blue-veth-a
  > sudo ip netns exec blue ip addr add 192.168.1.2/24 dev blue-veth-b
  > sudo ip netns exec blue ip link set dev blue-veth-b up
  > sudo ip link set dev blue-veth-a up
```
docker bridge network
```
  > docker network ls
  > brctl show
  > docker network inspect 37e137f568dc (network id)
  > exit
  > vagrant ssh default
  > docker run -d --name test1 busybox sh -c "while true; do sleep 2000; done"
  > ip link (now docker0 state up)
  > docker container inspect test1
  > docker exec -it test1 sh
  > ip a
  > exit
  > ping 172.17.0.2 (ping test1 container ip)
  > docker run -d --name test2 busybox sh -c "while true; do sleep 2000; done"
  > docker container inspect test2
  > ping 172.17.0.3 (ping test2 container ip)
  > sudo iptables --list -t nat
```
Container Name Link
```
  > docker container stop test2
  > docker container rm test2
  > docker run -d --name test2 --link test1 busybox sh -c "while true; do sleep 2000; done"
  > docker exec -it test2 sh
  > ping test1 (ping container name)
```