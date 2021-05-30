# bigdataDocker
# docker安装（在CentOS上安装Docker Engine）
https://docs.docker.com/engine/install/centos/
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