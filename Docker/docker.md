# 命令说明：
    -p 3306:3306：将容器的3306端口映射到主机的3306端口
    -v $PWD/conf/my.cnf:/etc/mysql/my.cnf：将主机当前目录下的conf/my.cnf挂载到容器的/etc/mysql/my.cnf
    -v $PWD/logs:/logs：将主机当前目录下的logs目录挂载到容器的/logs
    -v $PWD/data:/mysql_data：将主机当前目录下的data目录挂载到容器的/mysql_data
    -e MYSQL_ROOT_PASSWORD=123456：初始化root用户的密码
    -d imageId  : 镜像ID

    docker run --name mysql -p 3306:3306 -v D:/Professional/Mysql/conf/mysql_db.cnf:/etc/mysql/conf.d/mysql_db.cnf -v D:\Professional\Mysql\mysqldata:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -d 9546ca122d3a


	docker-compose.exe up -d


----

# 一 docker
1. docker官方镜像站:hub.docker.com
2. 镜像版本 ：parent ；base；common
3. 常用命令
docker logs -f -n
docker -help  查看全部命令
docker logs -help 查看。。。。
docker build -t -f  app:1.0.0
docker images  查看本地所有镜像
安装本地仓库:  docker run registory 
镜像贡献到社区： docker push
使用社区的镜像，获取镜像：docker pull   镜像名称
删除容器  docker remove     需要先 docker  stop
删除镜像  docker removei    需要先停止容器  再停止 镜像  再删除镜像

4. 镜像与容器   相当于  类与对象  的关系
5. docker 文章收藏
https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/
6. DockerToolBox中配置mysql 及 redis 过程
	1. 拉去镜像
从公司的仓库中拉去镜像
docker pull registry.saas.hand-china.com/tools/mysql:5.7.17
docker pull registry.saas.hand-china.com/tools/redis:3.2.6 
也可以直接拉去
docker pull mysql
docker pull redis
  	2. 执行镜像
docker run --name mysql -p 3306:3306 -e 
MYSQL_ROOT_PASSWORD=root  -d   9546ca122d3a
docker run --name redis -p 6379:6379 4e482b286430
	3. 登录mysql
mysql -h 172.16.16.45 -P 3306 -u root -p123
	4. 常用命令
docker ps 打印出正在运行的容器
docker ps -a 打印所有运行过的容器
docker-machine ls  查看容器基本信息
docker rm -f qgis-desktop-2-4
docker rm qgis-desktop-2-4 

7. DockerForWindows 版本安装
  http://docs.saas.hand-china.com/docs/hap-cloud/zh_CN/latest/developer-guide/backend/develop-env/install.html#docker-for-windows
8. 利用	commit	理解镜像构成
	1. 以定制一个	Web	服务器为例子，来讲解镜像是如何构建的
docker	run	--name	webserver	-d	-p	80:80	nginx
nginx镜像启动一个容器，命名为webserver，并且映射了	80 端口，这样我们可以用浏				览器去访问这个	nginx服务器
	2. 修改欢迎Docker的文字，使用	docker exec命令进入容器，修改其内容
以交互式终端方式进入webserver容器，并执行了bash命令，也就是获 得一个可操作的	Shell。然后，我们用	Hello,	Docker!	覆盖了/usr/share/nginx/html/index.html		的内容。再刷新浏览器的话，会发现内容被改变了。
	3. docker	diff	webserver 	命令看到具体的改动
	4. 构建新的镜像
docker commit --author "JiangP <peng.jiang@hand-chain.com>" --message "修改了默认网页"  webserver  nginx:v2
	5. 查看nginx镜像
docker images nginx
	6. 查看nginx镜像历史记录
docker	history	nginx:v2 
	7. 运行新的镜像
docker	run	--name	web2	-d	-p	81:80	nginx:v2
	8. 访问新的nginx服务器
http://localhost:81
注：使用docker  commit	命令虽然可以比较直观的帮助理解镜像分层存储的概念， 但是实际环境中并不会这样使用。不要使用docker	commit定制镜像，定制行为应该使用Dockerfile来完成。

9. 使用Dockerfile定制镜像
	1. 新建	Dockerfile文件，内容为：
FROM	debian:jessie
RUN	buildDeps='gcc	libc6-dev	make'	\				
&&	apt-get	update	\				
&&	apt-get	install	-y	$buildDeps	\				
&&	wget	-O	redis.tar.gz"http://download.redis.io/releases/redis3.2.5.tar.gz"	\				
&&	mkdir	-p	/usr/src/redis	\				
&&	tar	-xzf	redis.tar.gz	-C	/usr/src/redis	--strip-component s=1	\
&&	make	-C	/usr/src/redis	\				
&&	make	-C	/usr/src/redis	install	\				
&&	rm	-rf	/var/lib/apt/lists/*	\				
&&	rm	redis.tar.gz	\				
&&	rm	-r	/usr/src/redis	\				
&&	apt-get	purge	-y	--auto-remove	$buildDeps
	2. Dockerfile文件所在目录执行
docker	build	-t	nginx:v3. 
Docker进入容器命令
docker exec -ti mysql mysql -uroot -p