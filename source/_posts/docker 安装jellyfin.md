---
title: docker 安装jellyfin
date: 2022-04
tags: [docker, jellyfin]
---


# docker 安装jellyfin

---
### docker获取jellfin
    sudo docker pull jellyfin/jellyfin:latest
    
    sudo mkdir -p /srv/jellyfin/{config,cache}

    sudo docker run -d -v /srv/jellyfin/config:/config -v /srv/jellyfin/cache:/cache -v /media:/media --net=host jellyfin/jellyfin:latest

    sudo mkdir -p /media/jellyfin/

    sudo chown $USER. -R /media/jellyfin/

    sudo chmod 777 -R /media/jellyfin/

---
### docker 安装jellfin
参考：
[搭建Jellyfin影音服务器](https://post.smzdm.com/p/a7dz05ro/)

    docker run -d --name jellyfin -p 8096:8096 -v /jellyfin/config:/config -v /jellyfin/media:/media nyanmisaka/jellyfin

    -p 8096:8096 #影音服务器端口映射，左边8096可以修改其他端口，右边8096不能修改

    -v /jellyfin/config:/config #设置文件映射，还是左边/jellyfin/config可以自定义，右边/config不能修改

    -v /jellyfin/media:/media #媒体文件映射设置，左边/jellyfin/media可以选择自己的媒体库文件自定义（也可以搭建后在服务器中修改），右边/media不能修改

---
### 参数
    docker run -d --volume /path/to/config:/config --volume /path/to/cache:/cache --volume /path/to/media:/media --user 1000:1000 --net=host --restart=unless-stopped jellyfin/jellyfin

    -d: 
    This means that the Docker Container will run in the background. This can also be changed to -it this will make the container interactive allowing you to execute commands directly inside the container.

    --volume /path/to/config:/config: 
    This option connects the "/path/to/config" folder on your Host to the "/config" inside the container. You do not need to have any config present in /path/to/config as during the run command of the container Jellyfin will generate all of the necessary files. Everything in front of the colon is the of the config folder you've created in the previous step.

    --volume /path/to/cache:/cache: 
    This option connects the "/path/to/cache" folder on your Host to the "/cache" inside the container.

    --volume /path/to/media:/media: 
    This option connects the "/path/to/media" folder on your Host to the "/media" inside the container. This option has to be added in order for Jellyfin to find your media files. It is also possible that you have your media files spread across different places. In that case you can keep adding more volumes by using:

    --volume /path/to/anothermedia:/media2 & --volume /path/to/differentmedia:/media3 Enz.

    --user 1000:1000: 
    Is option for Ubuntu to specify that you want to run the Container as the current user. This command is not necessary if you are using Docker for Desktop

    -p 8096:8096: 
    This is an optional command to replace --net=host to change what port Jellyfin will be accessed from. If not specified Docker will run on port 8096 over http. This can be changed by changing the value in front of the colon, if you want it to run over port 80 you use: -p 80:8096.

    --net=host: 
    This option will tell the container to use the same network as the computer that it is running on. This means that if you have this running on your windows machine, you will be able to access jellyfin by using http://localhost:8096 instead of a different IP

    --restart=unless-stopped: 
    This option will make sure that whenever your computer starts, Jellyfin will also start. And that Jellyfin only stops when you give the docker stop command. If you do not want this, you can use different restart options:

    --restart=no: 
    do not automaticallu restart the container

    --restart=on-failure: 
    restart the container if it exits due to an error

    --restart=always: 
    always restart the container when it stops
