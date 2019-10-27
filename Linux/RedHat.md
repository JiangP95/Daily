## 1. 安转RedHat系统
## 2. yum资源替换 
    http://www.cnblogs.com/dafang/p/3566708.html
## RedHat安装yum
### 1. 卸载掉系统redhat自带的yum
	rpm -qa |grep yum |xargs rpm -e --nodeps
### 2. 下载相关的centos yum插件
    wget http://mirrors.163.com/centos/6/os/i386/Packages/python-iniparse-0.3.1-2.1.el6.noarch.rpm
      wget http://mirrors.163.com/centos/6/os/i386/Packages/yum-metadata-parser-1.1.2-16.el6.i686.rpm
      wget http://mirrors.163.com/centos/6/os/i386/Packages/yum-3.2.29-40.el6.centos.noarch.rpm
      wget http://mirrors.163.com/centos/6/os/i386/Packages/yum-plugin-fastestmirror-1.1.30-14.el6.noarch.rpm
### 3. 安装下载的yum包（如果没权限可以先修改权限 chmod 777 *）
    rpm -ivh python-iniparse-0.3.1-2.1.el6.noarch.rpm
    rpm -ivh yum-metadata-parser-1.1.2-16.el6.i686.rpm
    rpm -ivh yum3.2.2940.el6.centos.noarch.rpmyumpluginfastestmirror1.1.3014.el6.noarch.rpm
                最后两个需要一起安装，有相互依赖关系
    如果按照遇到问题：发现错误，python-urlgrabber版本必须大于等于3.9.1-10
             # rpm -ivh  yum-3.2.29-73.el6.centos.noarch.rpm yum-plugin-fastestmirror-1.1.30-	 37.el6.noarch.rpm
    warning: yum-3.2.29-73.el6.centos.noarch.rpm: Header V3 RSA/SHA1 Signature, key ID c105b9de: NOKEY
    error: Failed dependencies:
                python-urlgrabber >= 3.9.1-10 is needed by yum-3.2.29-73.el6.centos.noarch
    查看系统的python信息
    # rpm -qa|grep python
    发现python-urlgrabber-3.9.1-9.el6.noarch 版本低于3.9.1-10 
    卸载：python-urlgrabber-3.9.1-9.el6.noarch
     rpm -e python-urlgrabber-3.9.1-9.el6.noarch
    安装：rpm -ivh python-urlgrabber-3.9.1-11.el6.noarch.rpm
### 4. 下载配置文件CentOS6-Base-163.repo
 	wget http://mirrors.163.com/.help/CentOS6-Base-163.repo 
### 5. #vi CentOS6-Base-163.repo
    修改如下几处：
    [base]
    baseurl=http://tel.mirrors.163.com/centos/6/os/x86_64/
    [updates]
    baseurl=http://tel.mirrors.163.com/centos/6/os/x86_64/
    [extras]
    baseurl=http://tel.mirrors.163.com/centos/6/os/x86_64/
    [centosplus]
    baseurl=http://tel.mirrors.163.com/centos/6/os/x86_64/
    [contrib]
    baseurl=http://tel.mirrors.163.com/centos/6/os/x86_64/
### 6. 检查
	#rpm list|grep vim
### 7. 清除原有缓存
	yum clean all
### 8.重建缓存，以提高搜索安装软件的速度
	yum makecache
### 9.更新系统
	yum update



