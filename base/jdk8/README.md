# 添加jdk8
## 下载java8u271
https://download.oracle.com/otn/java/jdk/8u271-b09/61ae65e088624f5aaa0b1d2d801acb16/jdk-8u271-linux-x64.tar.gz?AuthParam=1609661797_99873039f523b7ec708ae98c2a1d0ab6
## 安装java
```
docker run -it dockerhubbigdata/bigdata:base bash
root@537b3c543495:~# mkdir -p /opt/modules && cd /opt/modules
root@537b3c543495:/opt/modules#

docker cp jdk-8u271-linux-x64.tar.gz 537b3c543495:/opt/modules/
root@537b3c543495:/opt/modules# tar -zxf jdk-8u271-linux-x64.tar.gz -C ./
root@537b3c543495:/opt/modules# rm jdk-8u271-linux-x64.tar.gz
```
## 保存镜像
```
docker commit --message "添加java8u271" 537b3c543495 dockerhubbigdata/bigdata:jdk8
```
## 测试
```
docker run -it dockerhubbigdata/bigdata:jdk8 bash
root@14e692c534be:~# vi HelloWorld.java
public class HelloWorld {
    public static void main(String args[]) {
        System.out.println("HelloWorld!");
    }
}
root@14e692c534be:~# /opt/modules/jdk1.8.0_271/bin/javac HelloWorld.java
root@14e692c534be:~# /opt/modules/jdk1.8.0_271/bin/java HelloWorld
HelloWorld!
```
