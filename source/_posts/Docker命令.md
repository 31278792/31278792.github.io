---
title: Docker命令
date: 2022-03-06 13:42:42
tags: 
	- docker
	- 命令
categories: Docker
password: 31278792
---
### docker安装
首先，简单说下docker容器安装步骤。  
**第一步，我们需要移除旧版本的docker命令：**  
```
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
```

<!--more-->

**第二步，安装所需依赖包：**  
```
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

**第三步，由于访问国外网站网速限制，我们需要配置镜像源，这里我用的是阿里云镜像：**  
```
sudo yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

**第四步，配置缓存：**  
```
sudo yum makecache fast
```

**第五步，执行docker安装命令：**  
```
sudo yum install -y docker-ce（这里docker-ce指的是社区版）
```

**第六步，配置镜像加速器，我用的是 [阿里云镜像加速器](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors)，登录自己阿里云账号获得自己专属镜像加速器地址：**  
```
suco mkdir -p /etc/docker(若没有该目录，则执行这条命令进行创建，有则跳过)
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": [这里输入你的阿里云镜像加速器地址]
}
EOF
```

**第七步，加载daemon配置文件：**  
```
sudo systemctl daemon-reload
```

**第八步，重启docker**  
```
sudo systemctl restart docker
```

### docker命令
`docker version`查看docker版本，用来检验docker是否安装成功  
`docker info`查看docker的信息，可以用来查看docker镜像加速器是否配置成功  
`systemctl start docker`开启docker  
`systemctl restart docker`重启docker
`systemctl enable docker`设置docker自启动  

#### docker镜像相关命令
`docker images`或`docker images ls`查看当前存在的镜像
`docker pull [镜像名]:[版本号]`获取镜像，不给版本号，默认最新latest  
`docker rmi [镜像ID]或[镜像名]`删除镜像，删除镜像时需确保该镜像没有创建容器  
`docker images -q`查看镜像ID  
`docker history [镜像名称]`查看一个镜像的制作历程  

#### docker容器相关命令
`docker ps`查看运行的容器  
`docker ps -a`查看所有的容器(包含运行和退出)  
`docker ps -a -q`查看所有容器的ID  
`docker run -i -t(-i -t用来作容器的交互) -d(后台运行容器) --rm(容器在启动后，执行完成命令或程序后自动销毁) --name(给容器自定义一个名字) -p(宿主机：内部端口)`运行容器  
实例：`docker run --rm -d --name tomcat123 -p 8081:8080 tomcat`  
`docker stop $(docker ps -a -q)`停掉所有容器  
`docker rm -f(强制)`删除容器  
`docker start`启动容器  
`docker restart`重启容器  
`docker exec`进入容器
实例：`docker exec -it tomcat-8080 bash`  
`exit`退出容器  
`docker cp`实现宿主机与容器之间文件交换  
`docker logs -f(查看实时日志) -t(查看日志产生日期) --tail=5(查看日志最后5行)`查看指定容器日志  

#### docker数据卷
数据卷可以在容器之间共享和重用，对数据卷的修改会立马生效，对数据卷的更新，不会影响镜像，数据卷具有持久化，即使容器被删除，也会一直存在，数据卷的两种创建方式  
`docker run -v(数据卷名称)`  
`docker run -v(路径)`  
例：`docker run -d --name tomcat-8080 -p 8080:8080 -v /home/yzw/docker-volumn/:/usr/local/tomcat/webapps/volumn tomcat`  
`