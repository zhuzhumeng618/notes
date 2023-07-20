# <p style ='background-color:purple;text-align:center;'><font color='white'>基本操作</font></p>

docker login

username: zhuzhumeng618 292267409@qq.com
password: Ss&200112

## <p style ='background-color:#8076a3;text-align:center;'><font color='white'>安装</font></p>

查看内核版本：

linux 操作系统 Centos7，linux 3.10 内核，docker 官方说至少 3.8 以上（Ubuntu 下要 linux 内核 3.8 以上）

```
[root@localhost ~]# uname -a
Linux msr-server 3.10.0-514.26.2.el7.x86_64 #1 SMP Tue Jul 4 15:04:05 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
```

更新 yum 包：

```linux
[root@localhost ~]# yum update
```

安装需要的软件包：

yum-util 提供 yum-config-manager 功能，另外两个是 devicemapper 驱动依赖的（一般更新完都有了）

```
[root@localhost ~]# yum -y install gcc
[root@localhost ~]# yum -y install gcc-c++
[root@localhost ~]# yum install -y yum-utils device-mapper-persistent-data lvm2
```

设置 docker 源：

```
[root@localhost ~]# sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

查看 docker 版本：

查看仓库中 docker 版本，可以指定安装，不指定安装则默认安装最新版本

```
[root@localhost ~]# yum list docker-ce --showduplicates | sort -r
Repodata is over 2 weeks old. Install yum-cron? Or run: yum makecache fast
Loaded plugins: fastestmirror
Installed Packages
docker-ce.x86_64            3:19.03.5-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:19.03.5-3.el7                    @docker-ce-stable
docker-ce.x86_64            3:19.03.4-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:19.03.3-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:19.03.2-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:19.03.1-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:19.03.0-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.9-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.8-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.7-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.6-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.5-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.4-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.3-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.2-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.1-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.0-3.el7                    docker-ce-stable 
docker-ce.x86_64            18.06.3.ce-3.el7                   docker-ce-stable 
docker-ce.x86_64            18.06.2.ce-3.el7                   docker-ce-stable 
docker-ce.x86_64            18.06.1.ce-3.el7                   docker-ce-stable 
docker-ce.x86_64            18.06.0.ce-3.el7                   docker-ce-stable 
docker-ce.x86_64            18.03.1.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            18.03.0.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.12.1.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.12.0.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.09.1.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.09.0.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.06.2.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.06.1.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.06.0.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.03.3.ce-1.el7                   docker-ce-stable 
docker-ce.x86_64            17.03.2.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.03.1.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.03.0.ce-1.el7.centos            docker-ce-stable 
Determining fastest mirrors
Available Packages
```

安装 docker：

```
[root@localhost ~]# yum makecache fast
[root@localhost ~]# yum install docker-ce # 安装最新版
```

启动 docker：

加入开机启动，验证安装

```
[root@localhost ~]# systemctl start docker
[root@localhost ~]# systemctl enable docker
[root@localhost ~]# docker version
```

测试 docker：

```
[root@localhost ~]# docker run hello-world
[root@localhost ~]# docker images
```

卸载 docker：

```linux
[root@localhost ~]# yum list installed | grep docker
[root@localhost ~]# yum -y remove docker-ce.x86_64
...
```
## <p style ='background-color:#8076a3;text-align:center;'><font color='white'>中央仓库</font></p>

Docker 的中央仓库，就是存放镜像的网站。

官网：

镜像最全，但是国内受限，下载速度慢。

```
htts://hub.docker.com/
```

国内镜像：

网易蜂巢

```
http://c.163.com/hub
```

daoCloud ※

```
http://hub.daocloud.io/
```

搭建私服：

修改注册文件

修改 /etc/docker/daemon.json 文件，若文件不存在，则手动创建。

```
{
    "registry-mirrors":["https://docker-cn.com"],
    "insecure-registries":["ip:port"] // 替换相应的IP和端口即可
}
```

重启服务

```
[root@localhost ~]# systemctl daemon-reload
[root@localhost ~]# systemctl restart docker
```
## <p style ='background-color:#8076a3;text-align:center;'><font color='white'>镜像操作</font></p>

拉取镜像到本地：

```
[root@localhost ~]# docker pull 镜像名称[:tag]
[root@localhost ~]# docker pull daocloud.io/library/tomcat:8.5.15-jre8
```

查看全部本地的镜像：

```
[root@localhost ~]# docker images
```

删除本地镜像：

```
[root@localhost ~]# docker rmi 镜像的标识
[root@localhost ~]# docker rmi -f $(docker images -q) # 强制删除
```

镜像的导入导出：

将本地的镜像导出

```
[root@localhost ~]# docker save -o 导出的路径 镜像id
```

加载本地的镜像文件

```linux
[root@localhost ~]# docker load -i 镜像文件
```

修改镜像名称

```
[root@localhost ~]# docker tag 镜像id 新镜像名称:版本
```
## <p style ='background-color:#8076a3;text-align:center;'><font color='white'>容器操作</font></p>

运行容器：

简单操作

```
[root@localhost ~]# docker run 镜像的标识镜像名称[:tag]
```

常用的参数

-d：代表后台运行容器。

-p宿主机端口:容器端口，为了映射当前 Linux 的端口和容器的端口。

--name 容器名称：指定容器的名称。

```
[root@localhost ~]# docker run -d -p宿主机端口:容器端口 --name 容器名称 镜像的标识镜像名称[:tag]
```

重新启动容器

```
[root@localhost ~]# docker restart <container_id>
```

启动已有容器

```
[root@localhost ~]# docker start <container_id>
```

查看正在运行的容器

-a：查看全部的容器，包括没有运行的。

-q：只查看容器得到标识。

```
[root@localhost ~]# docker ps [-aq]
```

查看容器的日志

-f：可以滚动查看日志的最后几行。

```
[root@localhost ~]# docker logs -f 容器id
```

进入到容器内部

```
[root@localhost ~]# docker exec -it 容器id bash
```

删除容器

<font color='red'>删除容器前，需要先停止容器</font>

```
# 停止指定的容器
[root@localhost ~]# docker stop 容器id

# 停止全部容器
[root@localhost ~]# docker stop $(docker ps -aq)

# 删除指定容器
[root@localhost ~]# docker rm 容器id
[root@localhost ~]# docker rm -f $(docker ps -a -q) # 强制删除

# 启动容器
[root@localhost ~]# docker start 容器id
```

# <p style ='background-color:purple;text-align:center;'><font color='white'>应用</font></p>

## <p style ='background-color:#8076a3;text-align:center;'><font color='white'>MySQL</font></p>

```
[root@localhost ~]# docker run -d -p3306:3306 -e MYSQL_ROOT_PASSWORD=1234 --name mysql mysql的镜像名:tag
```

## <p style ='background-color:#8076a3;text-align:center;'><font color='white'>Redis</font></p>

```
[root@localhost ~]# docker run -d -p6379:6379 --name redis redis的镜像名:tag
[root@localhost ~]# docker exec -it redis redis-cli # 进入 redis 命令行
```


## <p style ='background-color:#8076a3;text-align:center;'><font color='white'>数据卷</font></p>

为了部署我们的 demo 工程，需要使用到 cp 的命令将宿主机内的 demo 文件复制到容器内部。

数据卷：将宿主机的一个目录，映射到容器的一个目录中，可以在宿主机中操作目录中的内容，那么容器内部映射的文件，也会跟着一起改变。

创建数据卷：

```
# 创建数据卷之后，默认会存放在一个目录下 /var/lib/docker/volumes/数据卷名称/_data
[root@localhost ~]# docker volume create 数据卷名称
```

查看数据卷的详细信息：

```
[root@localhost ~]# docker volume inspect 数据卷名称
```

查看全部数据卷：

```
[root@localhost ~]# docker volume ls
```

删除数据卷：

```
[root@localhost ~]# docker volume rm 数据卷名称
```

应用数据卷：

当你映射数据卷时，如果数据卷不存在。Docker 会帮你自动创建，会将容器内部自带的文件，存储在默认的存放路径中。

采用数据卷的方式，容器内部的文件会自动同步到数据卷中。

采用路径的方式，容器内部的文件不会同步到指定的路径下。

```
# 映射成功，会同步容器内部文件夹内容
[root@localhost ~]# docker run -v 数据卷名称:容器内部的路径 镜像id

# 除了上面的用法，还直接指定一个路径作为数据卷的存放位置。这个路径下是空的。
# 映射成功，不会同步容器内部文件夹内容
[root@localhost ~]# docker run -v 路径:容器内部的路径 镜像id
```

# <p style ='background-color:purple;text-align:center;'><font color='white'>自定义镜像</font></p>

## <p style ='background-color:#8076a3;text-align:center;'><font color='white'>Dockerfile</font></p>

Dockerfile 是一个包含用于组合镜像的命令的文本文档。可以使用在命令行中调用的任何命令。Docker 通过读取 Dockerfile 中的指令自动生成镜像。

docker build 命令用于从 Dockerfile 构建映像。可以在 docker build 命令中使用 -f 标志指向文件系统中任何位置的 Dockerfile。

```
[root@localhost ~]# docker build -f /path/to/a/Dockerfile
```

Dockerfile 基本结构：

1. 基础镜像信息
2. 维护者信息
3. 镜像操作指令
4. 容器启动时执行指令

Dockerfile 文件说明：

Docker 以从上到下的顺序运行 Dockerfile 的指令。

为了指定基本映像，第一条指令必须是 FROM。

一个声明以 # 字符开头则被视为注释。

可以在 Docker 文件中使用 RUN，CND，FROM，EXPOSE，ENV 等指令。

**FROM**

<font color='red'>指定基础镜像，必须为第一个命令</font>

```
格式：
	FROM <image>
	FROM <image>:<tag>
	FROM <image>@<digest> # digest 是 64 位 sha256 的哈希值。

示例：
	FROM mysql:5.6
```

**MAINTAINER**

维护者信息

```
格式：
	MAINTAINER <name>

示例：
	MAINTAINER XiaoMiFeng
	MAINTAINER XiaoMiFeng@aliyun.com
	MAINTAINER XiaoMiFeng <XiaoMiFeng>@aliyun.com
```

**RUN**

构建镜像时执行的命令

```
RUN 用于在镜像容器中执行命令，其有以下两种命令执行方式：
shell执行
格式：
    RUN <command>

exec执行
格式：
    RUN ["executable", "param1", "param2"]

示例：
    RUN ["executable", "param1", "param2"]
    RUN apk update
    RUN ["/etc/execfile", "arg1", "arg1"]
```

注：RUN 指令创建的中间镜像会被缓存，并会在下次构建中使用。如果不想使用这些缓存镜像，可以在构建时指定 --no-cache 参数，如：docker build --no-cache

**ADD**

将本地文件添加到容器中，tar 类型文件会自动解压（网络压缩资源不会被解压），可以访问网络资源，类似 wget

```
格式：
    ADD <src>... <dest>
    ADD ["<src>",... "<dest>"] 用于支持包含空格的路径

示例：
    ADD hom* /mydir/          # 添加所有以"hom"开头的文件
    ADD hom?.txt /mydir/      # ? 替代一个单字符,例如："home.txt"
    ADD test relativeDir/     # 添加 "test" 到 `WORKDIR`/relativeDir/
    ADD test /absoluteDir/    # 添加 "test" 到 /absoluteDir/
```

**COPY**

功能类似 ADD，但是不会自动解压文件，也不能访问网络资源。

**CMD**

构建容器后调用，也就是在容器启动时才进行调用。

```
格式：
    CMD ["executable","param1","param2"] (执行可执行文件，优先)
    CMD ["param1","param2"] (设置了ENTRYPOINT，则直接调用ENTRYPOINT添加参数)
    CMD command param1 param2 (执行shell内部命令)

示例：
    CMD echo "This is a test." | wc -
    CMD ["/usr/bin/wc","--help"]
```

注：CMD 不同于 RUN，CMD 用于指定在容器启动时所要执行的命令，而 RUN 用于指定镜像构建时所要执行的命令。

**ENTRYPOINT**

配置容器，使其可执行化。配合 CMD 可省去 “application”，只使用参数。

```
格式：
    ENTRYPOINT ["executable", "param1", "param2"] (可执行文件, 优先)
    ENTRYPOINT command param1 param2 (shell内部命令)

示例：
    FROM ubuntu
    ENTRYPOINT ["top", "-b"]
    CMD ["-c"]
```

注：ENTRYPOINT 与 CMD 非常类似，不同的是通过 docker run 执行的命令不会覆盖 ENTRYPOINT，而 docker run 命令中指定的任何参数，都会被当做参数再次传递给 ENTRYPOINT。Dockerfile 中只允许有一个 ENTRYPOINT 命令，多指定时会覆盖前面的设置，而只执行最后的 ENTRYPOINT 指令。

**LABEL**

用于为镜像添加元数据

```
格式：
    LABEL <key>=<value> <key>=<value> <key>=<value> ...

示例：
　　LABEL version="1.0" description="这是一个Web服务器" by="ghz"
```

注：使用 LABEL 指定元数据时，一条 LABEL 指定可以指定一或多条元数据，指定多条元数据时不同元数据之间通过空格分隔。推荐将所有的元数据通过一条 LABEL 指令指定，以免生成过多的中间镜像。

**ENV**

设置环境变量

```
格式：
    ENV <key> <value>  #<key>之后的所有内容均会被视为其<value>的组成部分，因此，一次只能设置一个变量
    ENV <key>=<value> ...  #可以设置多个变量，每个变量为一个"<key>=<value>"的键值对，如果<key>中包含空格，可以使用\来进行转义，也可以通过""来进行标示；另外，反斜线也可以用于续行

示例：
    ENV myName John Doe
    ENV myDog Rex The Dog
    ENV myCat=fluffy
```

**EXPOSE**

指定与外界交互的端口

```
格式：
    EXPOSE <port> [<port>...]

示例：
    EXPOSE 80 443
    EXPOSE 8080    EXPOSE 11211/tcp 11211/udp
```

注：EXPOSE 并不会让容器的端口访问到主机。要使其可访问，需要在 docker run 运行容器时通过 -p 来发布这些端口，或通过 -P 参数来发布 EXPOSE 导出的所有端口。

**VOLUME**

用于指定持久化目录

```
格式：
    VOLUME ["/path/to/dir"]

示例：
    VOLUME ["/data"]
    VOLUME ["/var/www", "/var/log/apache2", "/etc/apache2"
```

注：一个卷可以存在于一个或多个容器的指定目录，该目录可以绕过联合文件系统。

并具有以下功能：
1. 卷可以容器间共享和重用；
2. 容器并不一定要和其它容器共享卷；
3. 修改卷后会立即生效；
4. 对卷的修改不会对镜像产生影响；
5. 卷会一直存在，直到没有任何容器在使用它。

**WORKDIR**

工作目录，类似于 cd 命令

```
格式：
    WORKDIR /path/to/workdir

示例：
    WORKDIR /a  (这时工作目录为/a)
    WORKDIR b  (这时工作目录为/a/b)
    WORKDIR c  (这时工作目录为/a/b/c)
```

注：通过 WORKDIR 设置工作目录后，Dockerfile 中其后的命令 RUN、CMD、ENTRYPOINT、ADD、COPY 等命令都会在该目录下执行。在使用 docker run 运行容器时，可以通过 -w 参数覆盖构建时所设置的工作目录。

**USER**

指定运行容器时的用户名或 UID，后续的 RUN 也会使用指定用户。使用 USER 指定用户时，可以使用用户名、UID 或 GID，或是两者的组合。当服务不需要管理员权限时，可以通过该命令指定运行用户。并且可以在之前创建所需要的用户。

```
格式:
    USER user
    USER user:group
    USER uid
    USER uid:gid
    USER user:gid
    USER uid:group

示例：
	USER www
```

注：使用 USER 指定用户后，Dockerfile 中其后的命令 RUN、CMD、ENTRYPOINT 都将使用该用户。镜像构建完成后，通过 docker run 运行容器时，可以通过 -u 参数来覆盖所指定的用户。

**ARG**

用于指定传递给构建运行时的变量

```
格式：
    ARG <name>[=<default value>]

示例：
    ARG site
    ARG build_user=www
```

**ONBUILD**

用于设置镜像触发器

```
格式：
	ONBUILD [INSTRUCTION]

示例：
	ONBUILD ADD . /app/src
	ONBUILD RUN /usr/local/bin/python-build --dir /app/src

```

注：当所构建的镜像被用做其它镜像的基础镜像，该镜像中的触发器将会被钥触发。

## <p style ='background-color:#8076a3;text-align:center;'><font color='white'>自定义镜像</font></p>

nginx 实例：

```
# This my first nginx Dockerfile
# Version 1.0

Base images 基础镜像

FROM centos

#MAINTAINER 维护者信息
MAINTAINER ghz

#ENV 设置环境变量
ENV PATH /usr/local/nginx/sbin:$PATH

#ADD  文件放在当前目录下，拷过去会自动解压
ADD nginx-1.8.0.tar.gz /usr/local/  # ngix
ADD epel-release-latest-7.noarch.rpm /usr/local/  # 企业linux扩展包

#RUN 执行以下命令
RUN rpm -ivh /usr/local/epel-release-latest-7.noarch.rpm
RUN yum install -y wget lftp gcc gcc-c++ make openssl-devel pcre-devel pcre && yum clean all
RUN useradd -s /sbin/nologin -M www

#WORKDIR 相当于cd
WORKDIR /usr/local/nginx-1.8.0

RUN ./configure --prefix=/usr/local/nginx --user=www --group=www --with-http_ssl_module --with-pcre && make && make install
RUN echo "daemon off;" >> /etc/nginx.conf

#EXPOSE 映射端口
EXPOSE 80

```

构建镜像：

docker build 最后的点表示指定镜像构建过程中的上下文环境目录。

比如说 dockerfile 中的 copy./package.json /project，其实拷贝的并不是本机目录下的 package.json 文件，而是 docker 引擎中展开的构建上下文中的文件，所以如果拷贝的文件超出了构建上下文的范围，Docker引擎是找不到那些文件的。

. 代表 dockerfile 文件中命令的文件在何处开始。

. 也可以是 ./root 等路径。

```
[root@localhost ~]# docker build -t 镜像名称[:tag] -f Dockerfile 所在目录 .
```

# <p style ='background-color:purple;text-align:center;'><font color='white'>Docker compose</font></p>

## <p style ='background-color:#8076a3;text-align:center;'><font color='white'>下载安装 Docker-Compose</font></p>

之前运行一个镜像，需要添加大量的参数。

可以通过 Docker-Compose 编写这些参数。

Docker-Compose 可以帮助我们批量的管理容器。

只需要通过一个docker-compose.yml 文件去维护即可。

1. 去 github 官网搜索 docker compose,下载 1.24.1 版本的 Docker-Compose

```
https://github.com/docker/compose/releases/download/1.24.1/docker-compose-Linux-x86_64
```

2. 将下载好的文件，拖拽到 Linux 操作系统中。

3. 需要将 DockerCompose 文件的名称修改一下，基于 DockerCompose 文件一个可执行的权限

```
[root@localhost ~]# mv docker-compose-Linux-x86_64 docker-compose 
[root@localhost ~]# chmod 777 docker-compose
```

4. 方便后期操作，配置一个环境变量

5. 将 docker-compose 文件移动到了 /usr/local/bin,修改了 /etc/profile 文件，给 /usr/local/bin 配置到了 PATH 中

```
[root@localhost ~]# mv docker-compose /usr/local/bin 
[root@localhost ~]# vi /etc/profile
export PATH=$JAVA_HOME:/usr/local/bin:$PATH 
source /etc/profile
```

6. 测试一下，在任意目录下输入 docker-compose

## <p style ='background-color:#8076a3;text-align:center;'><font color='white'>Docker-Compose 管理 MySQL</font></p>

```
version: '3.8' 
services: 
  mysql:                     # 服务的名称
    restart: always          # 代表只要Docker启动，那么这个容器就跟着一起启动
    image: daocloud.io/library/mysql:5.7.5-m15     # 指定镜像路径
    container_name: mysql    # 指定容器名称
    ports:
      - 3306:3306        # 指定端口号的映射
    environment:
      MYSQL_ROOT_PASSWORD: 123456         # 指定MySQL的ROOT用户登录密码
      TZ: Asia/Shanghai                 # 指定时区
    volumes:
      - /home/docker_mysql/:/var/lib/mysql        # 映射数据卷
  tomcat:
    restart: always          # 代表只要Docker启动，那么这个容器就跟着一起启动
    image: daocloud.io/library/tomcat:8.5.15-jre8     # 指定镜像路径
    container_name: tomcat    # 指定容器名称
    ports:
      - 8080:8080
    environment:
      TZ: Asia/Shanghai
    volumes:
      - /home/tomcat_webapps:/usr/local/tomcat/webapps
      - /home/tomcat_logs:/usr/local/tomcat/logs
```
## <p style ='background-color:#8076a3;text-align:center;'><font color='white'>使用 docker-compose 命令管理容器</font></p>

基于 docker-compose.yml 启动管理的容器：

```
[root@localhost ~]# docker-compose up -d
```

关闭并删除容器：

```
[root@localhost ~]# docker-compose down
```

开启关闭重启已经存在的由docker-compose维护的容器：

```
[root@localhost ~]# docker-compose start|stop|restart
```

查看由docker-compose管理的容器：

```
[root@localhost ~]# docker-compose ps
```

查看日志：

```
[root@localhost ~]# docker-compose logs -f
```

## <p style ='background-color:#8076a3;text-align:center;'><font color='white'>docker-compose 结合 Dockerfile 使用</font></p>

使用 docker-compose.yml 文件，以及 Dockerfile 文件在生成自定义镜像的同时启动当前镜像，并且由 docker-compose 去管理容器。

```
# yml文件
version: '3.8'
services:
  web-demo:
    restart: always
    build:                           # 构建自定义镜像
      context: ../                   # 指定Dockerfile文件所在路径
      dockerfile: Dockerfile         # 指定Dockerfile文件名称
    image: demo:1.0
    container_name: demo
    ports:
      8081:8080
    environment:
      TZ: Asia/Shanghai
```

Dockerfile 文件

```
FROM daocloud.io/library/tomcat:8.5.15-jre8
ADD demo.war /usr/local/tomcat/webapps
```

<font color='red'>docker-compose & dockerfile 联合使用时一定要注意 docker-compose 是否需要使用数据卷，如果使用了，则在 dcokerfile 中的 ADD & COPY 不会生效</font>

