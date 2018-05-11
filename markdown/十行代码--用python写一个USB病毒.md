大家好，我又回来了。

转至知乎

昨天在上厕所的时候突发奇想，当你把usb插进去的时候，能不能自动执行usb上的程序。查了一下，发现只有windows上可以，具体的大家也可以搜索（搜索关键词usb autorun）到。但是，如果我想，比如，当一个usb插入时，在后台自动把usb里的重要文件神不知鬼不觉地拷贝到本地或者上传到某个服务器，就需要特殊的软件辅助。



于是我心想，能不能用python写一个程序，让它在后台运行。每当有u盘插入的时候，就自动拷贝其中重要文件。



如何判断U盘的插入与否？


首先我们打开电脑终端，进入/Volumes目录，这时候插入U盘，可以发现它被挂载在了这个目录之下，也就是说，我们只要在固定时间扫描这个目录，当这个目录有新文件夹出现的时候，很可能有U盘被插入了。

我的设计是这样的，用time.sleep(3)函数，让程序保持运行状态，并且每隔三秒查看一下/Volumes/目录，如果多出来文件夹，就将其拷贝到另外的文件夹。

# encoding=utf-8
from time import sleep
import os, shutil
usb_path = "/Volumes/"
content = os.listdir(usb_path) # os.listdir(路径)返回路径下所有文件以及文件夹的名称
while True:
    new_content = os.listdir(usb_path) #每隔三秒扫描一次/Volumes/
    if new_content != content: # 如果发现异常，即多出一个文件夹，则退出
        break;
    sleep(3)
x = [item for item in new_content if item not in content]
# 找到那个新文件夹，返回包括新文件夹string类型名称的列表，这个表达方法很pythonic
shutil.copytree(os.path.join(usb_path, x[0]), '/Users/home/usb_copy')
# shutil.copytree 把目录下所有东西一股脑复制进/Users/home/usb_copy, 
# 放进了自己的home目录下
就像标题所示，我们真的只用了10行（其实是11行，凑个整：）完成了这个“病毒”。我们可以发现usb中的目录，在插入半分钟后全部躺在了home目录下了。



如何选择性的复制文件？

刚刚我们写了一个很简易的脚本测试了一下这个想法的可行性，但是还是有问题。刚才之所以能把U盘中所有文件很快复制进去，是因为U盘中只有两三个文件，大小不超过15M。如果目标U盘中有很多电影，音乐，这些我们并不需要的文件，我们的程序就应该能跳过它们，仅仅选择一些重要的比如.docx比如.ppt文件，或者仅仅复制最近修改过的那些文件，或者排除所有大小大于5M的文件。我们可以用python做到吗？当然！



os.walk 递归文件夹中所有文件

Python os.walk() 方法
​www.runoob.com
这里我放了一个别人的教程。大家可以大概了解一下，总之我大概理解是这么个东西。
还是举个例子吧。

我在某目录下创建了testwalk文件夹，里面有file123.txt三个文件，folder123三个文件夹，其中folder1中有文件file4.txt以及folder4

➜  testwalk touch file1.txt file2.txt file3.txt
➜  testwalk mkdir folder1 folder2 folder3
➜  testwalk cd folder1
➜  folder1 touch file4.txt && mkdir folder4
➜  folder1 cd ..
# 上面创建了这些文件以及文件夹，你也可以在图形界面上创建
# tree 是个很好玩的命令，可以直观地显示文件路径
➜  testwalk tree ./
testwalk/
├── file1.txt
├── file2.txt
├── file3.txt
├── folder1
│   ├── file4.txt
│   └── folder4
├── folder2
└── folder3

4 directories, 4 files
现在我们来测试一下

import os
for root, dirs, files in os.walk("./testwalk/"):
    for name in files:
        print(os.path.join(root, name))
    for name in dirs:
        print(os.path.join(root, name))
-----------------------------------------------------------------------------
运行结果：
./testwalk/folder1/file4.txt
./testwalk/folder1/folder4
./testwalk/file2.txt
./testwalk/file3.txt
./testwalk/file1.txt
./testwalk/folder2
./testwalk/folder3
./testwalk/folder1
root存放的是当前位置，它会把./testwalk/下所有的文件夹作为根目录，往下搜索

for root, dirs, files in os.walk("./testwalk/", topdown=False):
    print(root)
./testwalk/folder2
./testwalk/folder3
./testwalk/folder1/folder4
./testwalk/folder1
./testwalk/
单独查看 dirs

for root, dirs, files in os.walk("./testwalk/"):
    for name in dirs:
        print(os.path.join(root, name))

./testwalk/folder2
./testwalk/folder3
./testwalk/folder1
./testwalk/folder1/folder4
单独查看 files

for root, dirs, files in os.walk("./testwalk/", topdown=False):
    for name in files:
        print(os.path.join(root, name))

./testwalk/file2.txt
./testwalk/file3.txt
./testwalk/file1.txt
./testwalk/folder1/file4.txt
好了，我们现在需要递归usb文件夹，找到所有的file，查看大小，如果小于，比如3M，就拷贝进home，大于就舍去。

shutil模块

import shutil
>>> help(shutil)
>>> dir(shutil)
['Error', 'ExecError', 'SpecialFileError', 'WindowsError', 
'_ARCHIVE_FORMATS', '_BZ2_SUPPORTED', '_ZLIB_SUPPORTED', '__all__',
 '__builtins__', '__doc__', '__file__', '__name__', '__package__',
 '_basename', '_call_external_zip', '_destinsrc', '_get_gid', '_get_uid',
 '_make_tarball', '_make_zipfile', '_samefile', 'abspath', 
'collections', 'copy', 'copy2', 'copyfile', 'copyfileobj',
 'copymode', 'copystat', 'copytree', 'errno', 'fnmatch', 
'get_archive_formats', 'getgrnam', 'getpwnam', 'ignore_patterns', 
'make_archive', 'move', 'os', 'register_archive_format', 'rmtree', 
'stat', 'sys', 'unregister_archive_format']
好吧，看不懂，还是得看官方文档。

现在我们拿刚才的文件夹举例子，如果想把file1.txt拷贝到folder2：

>>> shutil.copy2('./file1.txt', './folder2')
------------------------------------------------我是分割线-----------
➜  folder2 ls
file1.txt
还有许多使用工具在shutil里面这里就不详述了。

os.path.getsize()判断大小

os.path.getsize(文件名）返回的是一个单位为byte的数值，如果用来查看文件大小，我们则需要手动写一个函数，将其换算成容易阅读的形式。

movie = /Users/home/somemovie.rmvb
def convert_bytes(num):
#   this function will convert bytes to MB.... GB... etc
    for x in ['bytes', 'KB', 'MB', 'GB', 'TB']:
        if num < 1024.0:
            return "%3.1f %s" % (num, x)
        num /= 1024.0

def getDocSize(path):
    try:
        size = os.path.getsize(path)
        return size
    except Exception as err:
        print(err)
print(convert_bytes(getDocSize(movie)))
结果：
1.3 GB
[Finished in 0.1s]
这里我们只要选择文件大小小于3M的即可，3M = 3 * 1024kB = 3 * 1024*1024byte

for root, dirs, files in os.walk(os.path.join(usb_path, x[0])): #MyUSB location
    for name in files:
        file = os.path.join(root, name)
        if os.path.getsize(file) < 3*1024*1024:
            shutil.copy2(file, target_folder)
结合shutil.copy2就可以把选定大小的文件复制进我们的目标文件夹了

如何指定文件类型

这里就需要正则表达式来帮助我们了。

正则表达式内容很多，《python核心编程》中用了整整一章来讲，所以我们也不深入了。下面是官方文档，感兴趣的可以看一下。

7.2. re - Regular expression operations - Python 2.7.14 documentation
​docs.python.org
如下，我们让指定文件后缀以及指定文件大小可以复制进我们的目标文件：

别忘了导入 re

import re
...
regex_filename = re.compile('(.*zip$)|(.*rar$)|(.*docx$)|(.*ppt$)|(.*xls$)')
for root, dirs, files in os.walk(os.path.join(usb_path, x[0])): #MyUSB location
    for name in files:
        file = os.path.join(root, name)
        if regex_filename.match(file) and os.path.getsize(file) < 1024*1024:
            shutil.copy2(file, target_folder)
用更加复杂的正则表达式可以更好地指定文件类型

根据修改时间筛选文件

>>> from os.path import *
>>> help(getmtime)
getmtime(filename)
    Return the last modification time of a file, reported by os.stat().

>>> help(getctime)
getctime(filename)
    Return the metadata change time of a file, reported by os.stat().
这时候我在目录下创建了一个文件叫做newfile

>>> getctime("newfile")
1522746383.716875
# 我们可以看到返回的time是从某个时间到现在的秒数，如需阅读，我们需要time.ctime来转换
>>> import time
>>> time.ctime(1522746383.716875)
'Tue Apr  3 17:06:23 2018' # 这就是刚才创建的时间啦
>>> help(time.ctime)ctime(...) # 查看文档
    ctime(seconds) -> string

    Convert a time in seconds since the Epoch to a string in local time.
    This is equivalent to asctime(localtime(seconds)). When the time tuple is
    not present, current time as returned by localtime() is used.
总之，对每一个文件进行修改时间的筛选可以只复制那些近期，或者特定时期修改或者添加过的文件，这个功能在特定情况下很有用。



总结

其实，标题这么起只是为了吸引大家注意，这就是一个小程序，也谈不上病毒。我更想通过这个例子，展示python对于文件处理的强大能力，引发大家的学习热情。以上实现都是基于macos，linux应该一样，windows稍加修改也可以成功。
