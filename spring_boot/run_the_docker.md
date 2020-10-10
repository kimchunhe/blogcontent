Java项目发布到Docker

1. 项目的根目录创建名为Dockerfile的无扩展名文件
``` tree
+-- src
|   +-- main
|   |   +-- java
|   +-- test
+-- target
+-- Dockerfile
+-- pom.xml
```
Dockerfile
``` bash
FROM openjdk:8-jdk-alpine
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```
2. 编译项目
``` bash
$ mvn clean install -DskipTests
```
3. 创建docker镜像
``` bash
$ docker build -t springio/spring-rest-api .
```