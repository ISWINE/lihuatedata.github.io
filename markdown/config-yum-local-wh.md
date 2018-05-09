###config-yum-local-wh
###配置本地镜像仓库
挂载CDROM
配置yum仓库 pwd
![avatar](http://oyu299liy.bkt.clouddn.com/17-11-3/68333579.jpg)
```
[root@16071224 CentOS_6.9_Final]# pwd #查看当前路径
[root@16071224 CentOS_6.9_Final]# cp Packages/ /  #复制Packages到根目录
```
###利用rpm安装yum管理软件createrepo
```
[root@16071224 Packages]# cd Packages
[root@16071224 Packages]# rpm -ivh createrepo-0.9.9-26.el6.noarch.rpm python-deltarpm-3.5-0.5.20090913git.el6.i686.rpm deltarpm-3.5-0.5.20090913git.el6.i686.rpm #根据提示输入python-deltarpm deltarpm 依赖
```
###建立软件包目录索引到repodata里 repodata自动在上级目录生成
```
[root@16071224 Packages]# cd ..
[root@16071224 home]# ls
78.jpg  a  a~  aaa.sh  git  hexo  Packages  vmtools
[root@16071224 home]# create
create-branching-keyboard  create-jar-links
create-cracklib-dict       createrepo
[root@16071224 home]# createrepo -v Packages/ #
```
###在etc/yum.repos.d/下建立配置文件myYUM.repo
```
cp yum.repos.d/* home/yumbak #备份
rm -rf yum.repos.d/*
vim myYUM.repo # 写入如下配置
[base]
name=mybase #仓库名
#mirrorlist=http://mirrorlist.centos.org/?release=
#$releasever&arch=$basearch&repo=os&infra=$infra
baseurl=file:///home/Packages #仓库地址
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
entabled=1
#released updates 
:wq  保存
[root@16071224 hexo]# yum clean all  #清楚缓存
已加载插件：fastestmirror, refresh-packagekit, security
Cleaning repos: base epel
清理一切
Cleaning up list of fastest mirrors
[root@16071224 hexo]# 
```
###安装结束
可以安装如下如下软件测试
```
```bash
tigervnc, gimp, bind, gcc, vsftp, minicom .....
```
![avatar](http://oyu299liy.bkt.clouddn.com/17-11-3/17448591.jpg)
