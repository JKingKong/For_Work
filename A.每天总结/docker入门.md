# **docker入门：**

参考：

https://zhuanlan.zhihu.com/p/23599229

https://zhuanlan.zhihu.com/p/30713987

 

# **一、镜像**

### **1、在DockerHub仓库搜索镜像:**

docker search 镜像名

### **2、拉取镜像到本地：**

docker pull 镜像名

### **3、列出本地镜像的所有信息：**

docker images

# **二、创建容器（利用镜像）**

### **1、根据一个镜像创建、启动、进入容器**:

docker run -it centos:latest /bin/bash

#### **在容器内：**

exit                #退出容器 并且容器进入Exited状态  

 

键盘快捷键：ctrl + p + q 			 #退出容器 并且容器依然处于Up状态

​			

 

## **2、以守护进程的方式创建一个容器:**

docker run -d centos:latest /bin/bash

 

# **三、本地容器信息：**

## **1、列出所有容器的信息：**

docker ps -a

## **2、列出所有容器的id：**

docker ps -q

 

## **3、查看正在运行的容器**

docker ps

# **四、启动容器**

## **1、启动指定容器：**

docker start container_id or container_name  # 可以同时输入多个id name

 

## **2、启动所有容器：**

docker start $(docker ps -a -q)

 

# **五、进入容器：**

## **1、进入一个启动的容器：**

docker attach container_id or container_name    

 

# **六、停止容器****1、停止指定容器：** 

docker stop container_id or container_name

 

## **2、停止所有容器：**

docker stop $(docker ps -a -q)

# **七、删除容器**

## **1、删除指定容器：**

docker rm container_id or container_name

 

## **2、删除所有非Up状态的容器：**

docker rm $(docker ps -a -q)

 

 

 

 

 

 

# **八、容器迁移**

参考：

https://blog.csdn.net/LEoe_/article/details/78685156

## **1、导出容器（不管处于什么状态）：**

docker export -o 导出后的文件.tar container_id or container_name

or

docker export container_id/container_name > 文件名.tar  

 

例：

docker export -o test_export.tar  0e88b871c28b
docker export 0e88b871c28b > test2_export.tar

 

scp命令（将本地服务器的文件传送到指定的服务器）：

https://www.cnblogs.com/likui360/p/6011769.html

 

## **2、导入容器,使其成为镜像：**

导入的文件可以使用docker import 命令导入变成镜像，该命令的格式为：

docker import [-c|--change[=[]]] [-m|--message[=MESSAGE]] file|URL|-[REPOSITORY[:YAG]

 

例：

docker import  test_export.tar  test/import:0.0.1

![img](file:///C:\Users\剑皇\AppData\Local\Temp\ksohtml9440\wps1.jpg) 

 

另外 docker load 命令也可以导入一个镜像存储文件，但是跟docker import 命令是有区别的：

 

**docker import：**丢弃了所有的历史记录和元数据信息，仅保存容器当时的快照状态。在导入的时候可以重新制定标签等元数据信息。

**docker load：**将保存完整记录，体积较大。

# **九、删除镜像**

## **1、删除镜像（先删除依赖于这个镜像的容器才行）：**

docker rmi image_id

 

## **2、删除所有镜像（先删除依赖于这些镜像的容器才行）**

docker rmi $(docker images -a -q)

 

 

 

# **十、创建镜像**

## **1、将容器转化为一个镜像：**

 docker commit -m "centos with git" -a "qixianhu" 72f1a8a0e394 xianhu/centos:git

其中，-m指定说明信息；-a指定用户信息；72f1a8a0e394代表容器的id；xianhu/centos:git指定目标镜像的用户名、仓库名和 tag 信息。注意这里的用户名xianhu，后边会用到。

 

## **2、利用Dockerfile创建镜像：**

Dockerfile:

\# 说明该镜像以哪个镜像为基础

FROM centos:latest

 

\# 构建者的基本信息

MAINTAINER xianhu

 

\# 在build这个镜像时执行的操作

RUN yum update

RUN yum install -y git

 

\# 拷贝本地文件到镜像中

COPY ./* /usr/share/gitdir/

 

docker build -t="xianhu/centos:gitdir" .

 

 

其中-t用来指定新镜像的用户信息、tag等。最后的点 ” . ”表示在当前目录寻找Dockerfile。

 

# **Docker中关于仓库的基本操作**

参考：https://zhuanlan.zhihu.com/p/23599229

 

Docker官方维护了一个DockerHub的公共仓库，里边包含有很多平时用的较多的镜像。除了从上边下载镜像之外，我们也可以将自己自定义的镜像发布（push）到DockerHub上。

 

在镜像操作章节中，我们新建了一个xianhu/centos:git镜像。

 

## **（1）访问https://hub.docker.com/，如果没有账号，需要先注册一个。**

 

## **（2）利用命令docker login登录DockerHub，输入用户名、密码即可登录成功：**

 

[root@xxx ~]# docker login

Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.

Username: xianhu

Password:

Login Succeeded

## **（3）将本地的镜像推送到DockerHub上，这里的xianhu要和登录时的username一致：**

 

[root@xxx ~]# docker push xianhu/centos:git    # 成功推送

[root@xxx ~]# docker push xxx/centos:git    # 失败

The push refers to a repository [docker.io/xxx/centos]

unauthorized: authentication required

## **（4）以后别人就可以从你的仓库中下载合适的镜像了。**

 

[root@xxx ~]# docker pull xianhu/centos:git

## **对应于镜像的两种创建方法，镜像的更新也有两种：**

1、创建容器之后做更改，之后commit生成镜像，然后push到仓库中。

2、更新Dockerfile。在工作时一般建议这种方式，更简洁明了。

 