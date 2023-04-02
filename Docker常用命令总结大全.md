## 1、docker基础操作
```shell
1.启动docker
  systemctl start docker
2.停止docker
  systemctl stop docker
3.查看当前docker运行的container
  docker ps
4.查看当前已经拉取得镜像
  docker images
5.搜索镜像
  docker search tomcat
6.拉取一个tomcat镜像
  docker pull tomcat
7.后端运行一个名字叫mytomcat容器 映射端口为8080
  docker run -d --name mytomcat -p 8080:8080 tomcat
8.删除某个images
  docker rmi imagesname
9.强制删除某个images
  docker rmi -f imagesname
10.删除所有container 
  docker rm -f $(docker ps -a)
11.docker查看容器详细信息：
  docker inspect container
12.docker查看监控命令 
  docker stats
13.container内存限制
  --memory 100M
14.containercpu权重设置
  --cpu-shares 10
15.查看日志
docker logs  e7a82f4fe0cf  2>&1 | grep "Bootstrap Password:"
docker logs containerid -f --tail=100
```
## 2、docker container命令
```shell
1.进入运行某个container里面
  docker exec -it containerid /bin/bash
2.查看docker最近10次运行的container
  docker ps -n 10
3.删除某个container
  docker rm containername
4.启动停止某个container
  docker stop containerid  docker start containerid
5.查看某个container详细信息
  docker inspect containerid
6.列举所有容器
  docker container ls
6.images打成tag  
  docker tag containerid registry.cn-hangzhou.aliyuncs.com/rundreams/name:版本号
7.docker登录阿里云
  docker login --username=rundreams@yeah.net registry.cn-hangzhou.aliyuncs.com
8.本地推送版本到远程厂库
  docker push registry.cn-hangzhou.aliyuncs.com/rundreams/tomcat:版本号
```

## 3、相关命令解释
```shell
docker volume create portainer_data #创建容器空间

docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
```
关于启动容器相关解释：
```shell
-d
是为了让这个容器在后台运行
-p 9000:9000
这个是端口映射问题，把容器内9000端口映射到主机的9000端口
--name portainer
重命名容器的名字为portainer，默认的话是镜像的名字
--restart always
这个设置是当docker启动时自动启动这个容器
-v /var/run/docker.sock:/var/run/docker.sock
-v portainer_data:/data
这两个命令可以类似的理解为硬盘挂载，这样容器内的文件和主机是共享的；
portainer/portainer
这个是我们那个镜像的名字
```
