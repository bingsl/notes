

####debian

```
docker run -it --name bing_docker --hostname bing_docker --network host --privileged debian:bullseye /bin/bash

docker start -ia bing_docker


apt-get update
mac设置网络
https://www.cnblogs.com/luo-c/p/15830769.html

设置ssh
https://blog.csdn.net/liubaoxyz/article/details/123368338


https://blog.csdn.net/qq_21383435/article/details/105482127?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7EPayColumn-1-105482127-blog-122583848.pc_relevant_multi_platform_whitelistv1&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7EPayColumn-1-105482127-blog-122583848.pc_relevant_multi_platform_whitelistv1&utm_relevant_index=1

```



```
docker run -itd --privileged -p 40022:22 --name aiot_zhongjin 9aec5c5fe4ba /usr/sbin/init

docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock --restart=always -v portainer_data:/data portainer/portainer-ce

docker exec -it <container-id>  /bin/bash

docker ps -a

docker images

docker run -itd --privileged -p 10022:22 -p 13306:3306 -p 10080:80 -p 19991:9991 -p 11883:1883 --name aiot_zhongjin 3fdb9dfb1617 /usr/sbin/init
```

```
若要删除未使用的volume，可以使用内置程序。volume rm命令。
docker volume ls -qf dangling=true

删除：
docker volume rm $(docker volume ls -qf dangling=true)


只删除未使用的volume:
docker volume prune




```

```
删除容器与数据卷
可以在删除容器的时候使用 docker rm -v 这个参数。


停止容器
docker stop XXX
-v 参数用于删除数据卷
docker rm -v XXX


```





````
创建macvlan

docker network create -d macvlan --subnet=192.168.6.0/24 --gateway=192.168.6.1 -o parent=ens192 macvlan_net



````

#### 创建docker

```
第一个参数是docker的名字
第二个是ip最后一位

./newdocker7.sh test 193

```



```shell
#!/bin/sh

NAME=$1
IP=$2

VHOSTNAME=$1
IP=192.168.6.${IP}
NAME=${NAME}_${IP}

IMAGE=8512846d5ef0

docker create --name ${NAME}  --hostname $VHOSTNAME --network vlan1 --ip ${IP} --cap-add SYS_ADMIN --cap-add NET_ADMIN -v /sys/fs/cgroup:/sys/fs/cgroup:ro ${IMAGE}
docker start ${NAME}
docker update ${NAME} --restart=always
```

### 删除docker

```
#!/bin/sh

NAME=$1
IP=$2

IP=192.168.6.${IP}
NAME=${NAME}_${IP}

docker stop ${NAME}
docker rm ${NAME}
docker volume prune -f
```

