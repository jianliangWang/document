## docker file

### docker 镜像定制

1. 直接导出save
2. 自己编写一个dockerFile
   1. 镜像是多层存储，每一层在前一层的基础上修改。容器也是多层存储，以镜像为基础，在其基础上加一层作为容器运行时的存储层。
   2. 通过dockerfile中定义一系列的命令和参数构成脚步，然后这些命令应用于基础镜像，依次添加层，最终生成一个新的镜像。

### dockerfile主要组成部分：

> 基础镜像信息 FROM centos:6.8
> 制作镜像操作指令RUN yum install openssh
> 容器启动时执行指令 CMD ["命令"]

### dockerfile指令

> FROM这个镜像的妈妈是谁（指定基础镜像）
> MAINTAINER 告诉别人，谁负责它（指定维护者信息，可以没有）
> RUN 你想让它干啥（在命令前加上RUN即可）
> ADD 添加宿主机的文件到容器内，会自动解压
> COPY 作用和ADD是一样的，都是拷贝宿主机的文件到容器内。仅仅拷贝
> WORKDIR 就是 cd
> VOLUME 给她一个存放行李到地方（设置卷，挂载主机目录）
> EXPOSE 它要打开的门是啥（指定对外的端口），在容器内暴露一个窗口
> CMD 奔跑吧兄弟（指定容器启动后要干的事情）

### 快速

1. 创建dockerfile，注意文件名字必须是这个
   
   ```
   FROM nginx
   RUN echo '<meta charset=utf-8>学习运行nginx</meta>' >
   /usr/share/nginx/html/index.html
   ```

2. 构建docker
   
     docker -t name:tag build .

3. 修改镜像名字

4. 运行该镜像
     docker run -d -p 80:80 nginx

5. 测试

### 指令

ENTRYPOINT 和 CMD的区别
     Entrypoint和CMD一样，都是在指定容器启动程序以及参数
     当指定了 ENTRYPOINT之后，CMD的语义就有了变化
     而是把CMD的内容当作参数传递给ENITRYPOINT指令。

正确的给容器传递一个-i参数该怎么办？
curl -s http://xxxx.com -I

解决办法：

1. 给容器传入新的完整的命令
2. 正确的姿势应该是使用ENTRYPOINT
   在docker中不使用CMD了，使用成ENTIRYPOINT

> ARG和ENV指令
> 设置环境变量
>      ENV NAME="zs"
>      ENV AGE="18"
>      ENV MYSQL_VERSION=5.8

     使用时使用$NAME

> ARG和ENV一样，都是设置环境变量
> 区别在于EVN无论在镜像构建时，还是容器运行，该变量都可以使用
> ARG只是用于构建镜像需要设置的变量，容器运行时就消失了

> VOLUME
> 
> 容器在运行时，应该保证在存储层不写入任何数据，运行在容器内产生的数据，我们推荐挂载，写入到宿主机上，进行维护
>      VOLUME /data #将容器内的/data文件夹，在容器运行时该目录自动挂载为匿名卷，任何项该目录中写入数据的操作都不会被容器记录，保证容器存储无状态理念。
>      VLUME ['/data1','/data2']

1. 容器数据挂在的方式，通过docker+file，指定VOLUME目录
2. 通过docker run -v 参数，直接设置需要映射挂载的目录

EXPOSE
指定容器运行时对外提供的端口服务

- 帮助使用该镜像的人，快速理解该容器的一个端口业务
     docker port 容器
     docker run -p 宿主机端口:容器端口
     docker run -p #作用时随机宿主机端口:容器内端口

WORKDIR  
用于在dockerfile中，目录的切换，更改工作目录  
WORKDIR /opt

USER  
用于改变环境，用于切换用户

1. 停止启动
   
   ```
   docker start  
   docker stop
   ```

2. 进入容器内
   
   ```
   docker exec -it 容器id bash
   ```

3. 删除容器
   
   ```
   docker rm 容器id
   docker rm `docker ps -aq` #批量删除
   ```

4. 查看容器内进程信息命令
   
   ```
   docker top 容器id
   ```

5. 查看容器内资源
   
   ```
   docker stats 容器id
   ```

6. 查看容器的具体信息
   
   ```
   docker inspect 容器id
   ```

7. 获取容器ip地址，容器格式化参数
   
   ```docker
   docket inspect --format '{{.NetworkSettings.IPAddress}}' 容器id
   ```