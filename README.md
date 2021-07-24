# bigdataDocker
# docker安装
## 在 CentOS 上安装Docker Engine
https://docs.docker.com/engine/install/centos/
## 在 Ubuntu 上安装Docker Engine
https://docs.docker.com/engine/install/ubuntu/
# 安装Docker Compose
https://docs.docker.com/compose/install/
# 登录docker
```
docker login
Username: jinyumantang
```
# 创建大数据网络
docker network create --driver=bridge --subnet=172.168.0.0/24 bigdata