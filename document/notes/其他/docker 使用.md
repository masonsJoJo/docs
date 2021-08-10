### docker 使用

#### docker image

镜像下载网站 https://hub.docker.com/

---

- 镜像相关命令
  - Docker image pull image:version
  - docker image ls 
  - docker search alpine --filter "is-official=true" 查询官方镜像
  - `docker image rm`用于删除镜像。
  - docker inspect    查看镜像详细信息
  - docker rmi -f ... 强制删除镜像

- 容器操作

  docker -v         #查看版本 

  docker info     #查看docker信息 运行容器 

  docker run --name 容器名 -d -p 3306:3306 mysql  docker 启动容器 

  docker run image_name docker run -d -p 80:80 nginx:latest run（创建并运行一个容器）  

  ​	-d 放在后台  

  ​	-p 端口映射 :docker的容器端口 

  ​	-P 随机分配端口 

  ​	-v 源地址(宿主机):目标地址(容器) 

  docker run -it --name centos6 centos:6.9 /bin/bash

  ​	 	-it 分配交互式的终端  

  ​		--name 指定容器的名字 

  ​		-p 8080:80 端口映射

  ​		--privileged=true 授权

  ​	 	/bin/bash覆盖容器的初始命令 

  docker run image_name  启动容器*** 

  docker stop container_id  停止容器 

  docker kill container_name   杀死容器 

  docker ps (-a -l -q)    查看容器列表 

  docker container rm  'docker ps -a -q'   删除所有容器 

  docker rm -f  'docker ps -a -q`      #删除所有容器

   docker ps -a  #查看容器列表 

  docker exec -it 77cd6bef4dc9 /bin/bash   #进容器 

  docker start/stop container-id||container-name 开启/停止 指定容器id或者容器名称的容器 

  ### dockerFile 构建镜像

  docker commit -m "nginx" -a="matteo" 74e3 centos7-nginx:1.0 



## 创建自定义网络

> 使用体验不好

- 创建网桥

docker network create --subnet=10.0.0.0/24 docker-br0

- 删除网桥

  Docker network rm docker-br0

- 

