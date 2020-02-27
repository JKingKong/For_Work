## 常用docker命令

```shell
docker search 名称
docker pull  名称：版本号
docker images
docker ps
docker ps -a
docker run -p 服务器端口:镜像里的端口 --name 自定义名字 -d 镜像id

#启动某个容器:
docker start 容器id

#停止某个容器:
docker stop 容器id

#进入容器
docker exec -it mysql容器id bash



#退出容器: 
ctrl+P+Q
```

## 配置Mysql

```shell
#配置mysql：
docker run -p 3306:3306 --name 自定义名字 -e MYSQL_ROOT_PASSWORD=密码  -d 镜像id
#进入容器
docker exec -it mysql容器id bash

#登录mysql
mysql -u root -p 

#输入密码：你自己定义的密码

#退出mysql: 
ctrl+d
```

## 配置Redis

```shell
#配置redis
docker run -p 6379:6379 --name 自定义名字 --requirepass "密码" -d 镜像id
```

