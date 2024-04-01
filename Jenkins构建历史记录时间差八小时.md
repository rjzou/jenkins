# Jenkins构建历史记录时间差八小时

## 容器时间
### 通过配置映射/etc/localtime 保持主机的时间地区和容器时间地区一致

```
version: '3.8'
services:
  jenkins:
    image: jenkins/jenkins
    privileged: true
    user: root
    ports:
      - 8080:8080
      - 50000:50000
    container_name: jenkins
    volumes:
      - /home/jenkins/configuration:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
      - /etc/localtime:/etc/localtime

```

## 容器时间正常，jenkins构建历史记录异常
### 1.通过拷贝命令，将容器内的/etc/timezone拷贝到主机中

# 拷贝
```
docker cp jenkins:/etc/timezone ./timezone
```
# 编辑 vim或vi
```
vim ./timezone

```
# 将原来的文件末尾加8
```
Etc/UTC  -> Etc/UTC+8
```

### 2.修改timezone的映射

```
services:
  jenkins:
    image: jenkins/jenkins
    privileged: true
    user: root
    ports:
      - 8080:8080
      - 50000:50000
    container_name: jenkins
    volumes:
      - /home/jenkins/configuration:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
      - /etc/localtime:/etc/localtime
      - ./timezone:/etc/timezone
      
```

### 3.重新启动即可
上面是docker-compose的方式，docker run 也类似，只需要将对应的映射文件放上去即可，当然也可以直接通过修改容器内部的/etc/timezone的文件