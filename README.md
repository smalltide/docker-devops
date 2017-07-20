# docker-devops
Learn Docker and Devops from Udemy course

1. Docker技術入門與實戰
   https://www.udemy.com/docker-china

![alt text](https://github.com/smalltide/docker-devops/blob/master/screenshot.gif "docker-devops")

1. docker
2. docker-machine
3. virtualbox
4. vagrant
5. Docker Compose
6. Docker Swarm


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
create and use docker bridge
```
  > docker network list
  > docker network inspect bridge
  > brctl show
  > docker network create -d bridge my-bridge
  > docker network list
  > docker network inspect my-bridge
  > docker run -d --name test2 --network my-bridge busybox sh -c "while true; do sleep 2000; done"
  > docker network connect my-bridge test1
  > docker network inspect my-bridge
  > docker container exec test1 ip a
  > docker run -d --name test3 --network my-bridge busybox sh -c "while true; do sleep 2000; done"
  > docker container exec test3 ping test1
```
Container Port Mapping
```
  > docker image pull nginx
  > docker run -d --name ng1 nginx
  > docker container inspect ng1
  > docker run -d --name web -p 80:80 nginx
  > docker container list -a
  > curl 127.0.0.1
```
Docker host network
```
  > docker network ls
  > ip a
  > docker container stop web
  > docker container rm web
  > docker run -d --name web nginx
  > curl 127.0.0.1 
  > docker container stop web
  > docker container rm web
  > docker run -d --name web --network host nginx
  > docker network inspect host
  > curl 127.0.0.1
```
Dockerize a python flask web app
```
  > docker run -d --name redis redis
  > cd flask-redis
  > docker build -t smalltides/python-redis:latest .
  > docker run -d --name web --link redis -p 5000:5000 smalltides/python-redis
  > curl 127.0.0.1:5000
  > docker push smalltides/python-redis:latest
```
create two docker host using vagarant
```
  > cd multi-node-vagrant
  > vagrant up
  > vagrant status (see docker-node1 and docker-node2)
  > vagrant ssh docker-node1
  > sudo gpasswd -a ${USER} docker (Adding user ubuntu to group docker)
  > vagrant ssh docker-node2
  > sudo gpasswd -a ${USER} docker (Adding user ubuntu to group docker)
```
Multi-Host Network with ectd
```
  > (http://docker-k8s-lab.readthedocs.io/en/latest/docker/docker-etcd.html)
  > vagrant ssh docker-node1
  > wget https://github.com/coreos/etcd/releases/download/v3.0.12/etcd-v3.0.12-linux-amd64.tar.gz
  > tar zxvf etcd-v3.0.12-linux-amd64.tar.gz 
  > cd etcd-v3.0.12-linux-amd64
  > nohup ./etcd --name docker-node1 --initial-advertise-peer-urls http://192.168.205.10:2380 \
--listen-peer-urls http://192.168.205.10:2380 \
--listen-client-urls http://192.168.205.10:2379,http://127.0.0.1:2379 \
--advertise-client-urls http://192.168.205.10:2379 \
--initial-cluster-token etcd-cluster \
--initial-cluster docker-node1=http://192.168.205.10:2380,docker-node2=http://192.168.205.11:2380 \
--initial-cluster-state new&
  > vagrant ssh docker-node2
  > wget https://github.com/coreos/etcd/releases/download/v3.0.12/etcd-v3.0.12-linux-amd64.tar.gz
  > tar zxvf etcd-v3.0.12-linux-amd64.tar.gz
  > cd etcd-v3.0.12-linux-amd64
  > nohup ./etcd --name docker-node2 --initial-advertise-peer-urls http://192.168.205.11:2380 \
--listen-peer-urls http://192.168.205.11:2380 \
--listen-client-urls http://192.168.205.11:2379,http://127.0.0.1:2379 \
--advertise-client-urls http://192.168.205.11:2379 \
--initial-cluster-token etcd-cluster \
--initial-cluster docker-node1=http://192.168.205.10:2380,docker-node2=http://192.168.205.11:2380 \
--initial-cluster-state new&
  > ./etcdctl cluster-health
  > sudo service docker stop (in docker-node1 and docker-node2)
  > sudo /usr/bin/docker daemon -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock --cluster-store=etcd://192.168.205.10:2379 --cluster-advertise=192.168.205.10:2375& (in docker-node1)
  > sudo /usr/bin/docker daemon -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock --cluster-store=etcd://192.168.205.11:2379 --cluster-advertise=192.168.205.11:2375& (in docker-node2)
  > docker version (in docker-node1 and docker-node2)
  > docker network create -d overlay demo (in docker-node1, but because etcd, the network overlay demo sync to docker-node2)
  > docker network ls (in docker-node1 and docker-node2)
  > ./etcdctl ls /docker (in docker-node1 and docker-node2)
  > ./etcdctl ls /docker/nodes (in docker-node1 and docker-node2)
  > docker run -d --name test1 --net demo busybox sh -c "while true; do sleep 3600; done" (in docker-node1)
  > docker run -d --name test2 --net demo busybox sh -c "while true; do sleep 3600; done" (in docker-node2)
  > docker exec -it test1 ifconfig (10.0.0.2)
  > docker exec -it test2 ifconfig (10.0.0.3)
  > docker exec -it test2 ping 10.0.0.2 (in docker-node2)

```
Multi-Host Networking Overlay with Flannel
```
  > (http://docker-k8s-lab.readthedocs.io/en/latest/docker/docker-flannel.html)
```
Use Docker Compose to deploy Python Flask Redis Application
```
  > vagrant init ubuntu/trusty64
  > vagrant up
  > vagrant status
  > vagrant ssh default
  > install docker in ubuntu 14.04 vm (https://docs.docker.com/engine/installation/linux/ubuntu/#install-from-a-package)
  > curl -L https://github.com/docker/compose/releases/download/1.13.0/docker-compose-`uname -s`-`uname -m` > ./docker-compose (install docker-compose v1.13.0)
  > chmod +x docker-compose (change permission to executable)
  > sudo cp docker-compose /usr/local/bin/docker-compose
  > cd docker-compose-flask-redis
  > sudo docker-compose build
  > sudo docker image list
  > sudo docker-compose up
  > sudo docker-compose up -d (backound run)
  > sudo docker-compose stop
```
Use Docker Compose to Scaling and LoadBlancing
```
  > cd docker-compose-lb-scaling
  > sudo docker-compose build
  > sudo docker-compose up -d (backound run)
  > sudo docker container ls
  > curl 127.0.0.1
  > docker-compose scale -h
  > sudo docker-compose scale web=3
```
Use Docker Compose to deploy a complex voting app
```
  > (https://github.com/udemy-course/example-voting-app)
  > git clone git@github.com:udemy-course/example-voting-app.git
  > cd example-voting-app
  > sudo docker-compose build
  > sudo docker-compose up -d (backound run)
  > sudo docker container ls
```
create three docker host using vagarant
```
  > cd swarm-node-vagrant
  > vagrant up
  > vagrant status (see master, work1 and work2)
  > 
```
create three swarm docker host using docker-machine
```
  > docker-machine create swarm-manager
  > docker-machine create swarm-worker1
  > docker-machine create swarm-worker2
  > docker-machine ls
  > docker-machine env swarm-manager
  > docker-machine ssh swarm-manager
  > docker swarm init --advertise-addr=192.168.99.100
  > docker node ls
  > docker-machine ssh swarm-worker1
  > docker swarm join --token SWMTKN-1-0kg0b56j2xqitx3xh1uiv9sdvgvp97gr9736ubeaa28dq2qoet-21r255staeyu98v5xnxawxunr 192.168.99.100:2377
  > docker-machine ssh swarm-worker2
  > docker swarm join --token SWMTKN-1-0kg0b56j2xqitx3xh1uiv9sdvgvp97gr9736ubeaa28dq2qoet-21r255staeyu98v5xnxawxunr 192.168.99.100:2377
  > docker-machine ssh swarm-manager
  > docker node ls
```