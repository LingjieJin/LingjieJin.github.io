---
title: docker 安装
date: 2022-04
tags: [linux, ubuntu18.04, docker]
---

# 安装docker

参考:

[Docker（一）Docker简述及安装(ubuntu18.04)](https://www.cnblogs.com/lx--/p/14073513.html)

[Docker（二）Docker的镜像操作](https://www.cnblogs.com/lx--/p/14074544.html)

[Docker（三）Docker的容器操作](https://www.cnblogs.com/lx--/p/14075480.html)

[Docker（四）容器的导出与导入（迁移）](https://www.cnblogs.com/lx--/p/14086351.html)

[Docker（五）将自己的docker镜像发布到仓库](https://www.cnblogs.com/lx--/p/14086992.html)

### 安装docker


---
### docker 操作
#### 启动docker
    sudo service docker start

#### 停止docker
    sudo service docker stop

#### 重启docker
    sudo service docker restart

---
#### 列出镜像
    docker image ls

#### 拉取镜像
    docker image pull library/hello-world

#### 删除镜像
    docker image rm 镜像id/镜像ID

---
#### 创建容器
    docker run [选项参数] 镜像名 [命令]

#### 停止一个已经在运行的容器
    docker container stop 容器名或容器id

#### 启动一个已经停止的容器
    docker container start 容器名或容器id

#### kill掉一个已经在运行的容器
    docker container kill 容器名或容器id

#### 删除容器
    docker container rm 容器名或容器id



### 修改docker源
参考：
[ubuntu18.04安装docker-ce国内源](https://blog.csdn.net/omaidb/article/details/122062219)

#### 解决 docker: Error response from daemon: ... : net/http: TLS handshake timeout.的问题

国内docker源地址:
- 网易              http://hub-mirror.c.163.com
- 中国科技大学      https://docker.mirrors.ustc.edu.cn
- 阿里云            https://pee6w651.mirror.aliyuncs.com

1. 修改docker镜像源，如果没有 daemon.json就新建添加以下内容：
```bash
nano /etc/docker/daemon.json
{"registry-mirrors": ["http://hub-mirror.c.163.com","https://registry.docker-cn.com"]}
```

2. 重新加载daemon文件
    
        sudo systemctl daemon -reload

3. 重启docker服务

        sudo systemctl restart docker.service

4. 查看是否配置成功

        sudo docker info

5. 重启docker服务

        systemctl restart docker.service

6. 查看docker镜像

        docker info | grep Mirrors -A 1

7. 重启网络
    
        /etc/init.d/networking restart

---
### 问题

#### Errors were encountered while processing
    解决方法：
    cd /var/lib/dpkg
    sudo mv info info.bak
    sudo mkdir info
    sudo apt-get upgrade