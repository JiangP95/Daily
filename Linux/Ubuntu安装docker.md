# 1. Ubuntu安装docker
	命令  sudo apt install docker.io
# 2. 操作
    查看版本  docker version
    更新到最新版本
     curl -s https://get.docker.com|sh
    启动服务
    service docker start
    docker pull [options] name[:tag]  拉取镜像到本地
    options拉取参数选项 tag版本 省略表示最新版本
    docker images [options][repository[:tag]]   查看本地镜像
            repository[:tag]指定镜像以及版本
    默认下载镜像地址docker的仓库  hub.docker.com
    可以到网易蜂巢中获取镜像   https://c.163.com/ à镜像中心
    docker run [options] image[:tag][command][arg…] 运行镜像
       [command][arg…]命令以及参数
    docker run –-help 可以查看到参数
    docker run -d name  -d在后台运行
      dicker ps 查看本地已经在运行的镜像
      docker stop name 停止镜像
# 3. docker拉取镜像过程
    docker pull
    docker daemon先到检查本地仓库是否有该镜像，若没有则到远程仓库中拉取
    docker run
    docker daemon 先检查本地仓库是否有该镜像，若没有则到远程仓库中拉取，然后放在本地容器中运行
    docker rm **  删除镜像
# 4. Nginx镜像
     a. 拉取镜像
    docker pull hub.c.163.com/library/nginx:latest
     b. 查看镜像
     c. 运行镜像
        -d后台运行   返回一个运行容器的id
     d. 查看运行中的镜像
        可以看到运行的容器 对应id
     e. 查看容器内部
        类似一个linux系统
     f. 退出查看容器内部  exit
     g. 停止运行镜像
      docker stop e058
 
# 5. docker网络
  	启动镜像时指定主机与镜像的端口映射
	查看端口是否监听
# 6. 制作自己的镜像
     docker build 构建镜像
     Jpress    开源web
  	[下载jpress](http://jpress.io/)
  	https://github.com/JpressProjects/jpress/blob/master/wars/jpress-web-newest.war
 	docker build -t jpress:latest . //-t 镜像名与版本  .代表当前目录

 