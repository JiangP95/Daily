# 桌面系统安装

## U盘安装系统

	使用 软通牒 制作U盘 ，U盘启动安装系统
## 我的分区设置
    1. 挂载点/；主分区；安装系统和软件；大小为30G；分区格式为ext4； 
    2. 挂载点/home；逻辑分区；相当于“我的文档”；大小为硬盘剩下的; 分区格式ext4； 
    3. swap；逻辑分区；充当虚拟内存；大小等于内存大小（本人16G）；分区格式为swap 
    4. /boot ；引导分区；逻辑分区； 大小为1G ；分区格式为ext4；
    5 /tmp  5G  系统临时

## 安装搜狗输入法
    下载搜狗拼音ubuntu版本

    Ubuntu16.04版本，搜狗输入法不兼容，需要几个依赖包

    移除搜狗拼音：sudo apt remove sogoupinyin

    安装依赖的软件包：sudo apt install libopencc1 fcitx-libs fcitx-libs-qt fonts-droid-fallback

     重新安装搜狗拼音：sudo dpkg -i sogoupinyin_2.1.0.0082_amd64.deb

     注销或者重启系统，即可使用搜狗拼音

## 安装JDK
    使用源安装：
    sudo add-apt-repository ppa:webupd8team/java
        sudo apt-get update
        sudo apt-get install oracle-java8-installer
        sudo apt-get install oracle-java8-set-default
------

# 常用命令
	删除软件： sudo apt-get remove  软件名称
	卸载openjdk：sudo apt-get remove openjdk*
    apt-cache search # ------(package 搜索包)   
    apt-cache show #------(package 获取包的相关信息，如说明、大小、版本等)   
    sudo apt-get install # ------(package 安装包)   
    sudo apt-get install # -----(package - - reinstall 重新安装包)   
    sudo apt-get -f install # -----(强制安装?#"-f = --fix-missing"当是修复安装吧...)   
    sudo apt-get remove #-----(package 删除包)   
    sudo apt-get remove - - purge # ------(package 删除包，包括删除配置文件等)   
    sudo apt-get autoremove --purge # ----(package 删除包及其依赖的软件包+配置文件等（只对6.10有效，强烈推荐）)   
    sudo apt-get update #------更新源   
    sudo apt-get upgrade #------更新已安装的包   
    sudo apt-get dist-upgrade # ---------升级系统   
    sudo apt-get dselect-upgrade #------使用 dselect 升级   
    apt-cache depends #-------(package 了解使用依赖)   
    apt-cache rdepends # ------(package 了解某个具体的依赖?#当是查看该包被哪些包依赖吧...)   
    sudo apt-get build-dep # ------(package 安装相关的编译环境)   
    apt-get source #------(package 下载该包的源代码)   
    sudo apt-get clean && sudo apt-get autoclean # --------清理下载文件的存档 && 只清理过时的包   
    sudo apt-get check #-------检查是否有损坏的依赖   

    
    
    
    