###vmware-CentOS-install
###下载CentOS
[进入CentOS官网](https://www.centos.org/download/)找到中国服务器上的镜像wvc

![avatar](http://oyu299liy.bkt.clouddn.com/17-11-3/67032314.jpg)
```
http://mirrors.ustc.edu.cn/centos/6.9/isos/i386/CentOS-6.9-i386-bin-DVD1.iso #32位完整版系统包DVD1为软件包
```
###[下载安装VMware](http://wcy1.fgtrj.com/vmware12.zip)
```
A02H-AU243-TZJ49-GTC7K-3C61N 激活密钥
```
####一路Next到你需要的路径

###开始配置
```
VMware>创建新的主机>典型>稍后安装操作系统>linux>版本选择CentOS>自定义 virtual machine [name] or [Route]>定义磁盘大小[30G] or next>自定义硬件移除打印机 or 设置cd/dvd：使用iso镜像文件 选择刚才下载的CentOS-6.9-i386-bin-DVD1.iso  Next>完成
开启虚拟机>install or upgr*** system 按上下键选择 >
他问你是否要检查iso 选择skip>选择默认语言Chinese(simplified)简体中文>
美式英语>Next>是！忽略所有数据>Next>Next>输入root账户的密码>
创建自定义布局>标准分区/ext4/"boot"400mb or swap"2000"物理内存2倍 or ext4/"/"使用剩下所有内存>next>next>开始安装>虚拟机重新引导
前进>同意>不创建用户>选中在网络上同步时间与日期>不开启kdump>next
```
###配置完成
