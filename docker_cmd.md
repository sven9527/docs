#Docker
###Docker常用命令
    获取镜像：sudo docker pull ubuntu
    查看镜像：sudo docker images
    查找某个镜像：sudo docker search mysql
    查看本机上所有容器：sudo docker ps -a
    删除某个容器：sudo docker rm containerid
    删除某个镜像：sudo docker rmi [TAG] or IMAGEID
    强行删除某个镜像：sudo docker rmi -f [TAG] or IMAGEID

    列出虚悬镜像(旧镜像被覆盖掉名字、TAG)：sudo docker image ls -f dangling=true
    删除虚悬镜像：sudo docker image prune

    查看中间层镜像：sudo docker images -a
    中间层镜像为顶层镜像所依赖的镜像，无需手动删除，删除顶层镜像时会自动删除


###创建镜像：docker commit
	1.基于dockerfile
	sudo docker build -t home/zhangxing/PycharmProjects/fashion-analysis/
	2.基于已有镜像的容器;
	sudo docker commit -m "Added a new file" -a "xinzhang" c53e576a6b31 test01:ubuntu_01
	3.基于本地模板导入
	sudo cat ubuntu-14.04-x86_64-minimal.tar.gz | docker import - ubuntu:14.04

###镜像加TAG：
    sudo docker tag imageid repository:tag


###存出和载入镜像：
    1.存出：sudo docker save -o file name image
    2.载入：sudo docker load --input xxxx.tar
    3.上传镜像：
	    a.加TAG：sudo docker tag test:latest user/test:latest
	    b.上传：sudo docker push user/test:latest

###容器：
    1.创建容器：docker create [OPTIONS] IMAGE [COMMAND] [ARG...]
    2.创建并运行容器：sudo docker run ubuntu /bin/echo 'hello'
    3.守护态运行：sudo docker run -d ubuntu -c "it's a test"
    4.终止运行：sudo docker stop containerid
    5.重新启动：sudo docker start containerid


###进入容器：
    1.sudo docker attach xxxx
    2.sudo docker exec -ti xxxxx /bin/bash

###删除容器：
    sudo docker rm xxxx
    OPTIONS: 1. -f 强制终止并删除
         2. -l 删除容器连接，但保留容器
         3. -v 删除容器挂载的容器卷


###导入导出容器：
    1.导出（为文件）：sudo docker export containerid > xxxx.tar
    2.导入（为镜像）：cat xxxx.tar | sudo docker import - namexxx


###DockerFile
    DockerFile中每个指令都会建立一层，并在该层上执行这些命令，执行结束后，commit这一层的修改，构成新的镜像。
    ***因此不应该过多使用多行命令，这是完全没有意义的，而且很多运行时不需要的东西，都被装进了镜像里.


####Good Example:========>>>>>
    FROM debian:jessie
    
    RUN buildDeps='gcc libc6-dev make' \
        && apt-get update \
        && apt-get install -y $buildDeps \
        && wget -O redis.tar.gz "http://download.redis.io/releases/redis-3.2.5.tar.gz" \
        && mkdir -p /usr/src/redis \
        && tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1 \
        && make -C /usr/src/redis \
        && make -C /usr/src/redis install \
        && rm -rf /var/lib/apt/lists/* \
        && rm redis.tar.gz \
        && rm -r /usr/src/redis \
        && apt-get purge -y --auto-remove $buildDeps


=====================================


###Docker-compose
    负责实现对Docker集群的快速编排；定位：定义和运行多个Docker容器的应用.
    service:一个应用的容器，实际可以包含多个运行相同镜像的容器实例
    project:由一组关联的应用容器组成的一个完整业务单元，在docker-compose.yml文件中定义.
    
    1.一个标准的compose文件包含：version、service、networks三个部分
    ============sample===========
    version: '3'
    services:
      web:
        build: .
        ports:
          - "5000:5000"
        restart: "no"
    
      redis:
        image: "redis:3.0.7"
    =============================


    添加文件绑定挂载：
    ============sample===========
    version: '3'
    services:
      web:
        build: .
        ports:
          - "5000:5000"
        restart: "no"
        volumes:
          -
    
      redis:
        image: "redis:3.0.7"
    =============================



####Compose具有管理应用程序整个生命周期的命令：
    1.启动，停止和重建服务
    2.查看正在运行的服务的状态
    3.流式传输运行服务的日志输出
    4.在服务上运行一次性命令
	
