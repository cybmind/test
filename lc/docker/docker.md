docker-compose
Dockerfile
swarm


## docker-compose 安装
下载最新版的docker-compose文件 
```
sudo curl -L https://github.com/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```
添加可执行权限
```
sudo chmod +x /usr/local/bin/docker-compose
```

创建外部网络
```
docker network create esnet
```