##hexo-CentOS
####本次安装环境
CentOS 6.9 i386

####什么是 Hexo？
Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

##安装
安装 Hexo 只需几分钟时间，安装过程遇到问题或无法找到解决方式，请[提交问题](https://github.com/hexojs/hexo/issues)，

安装前提
安装 Hexo 相当简单。然而在安装前，您必须检查电脑中是否已安装下列应用程序：

[Node.js](https://lihuate.github.io/2017/11/04/nodejs-CentOS/)

[Git](https://lihuate.github.io/2017/11/03/githubdeploy/)

####所有必备的应用程序安装完成后，即可使用 npm 安装 Hexo。

``` 
$ npm install -g hexo-cli
```
###安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。
 ```
```$ hexo init <folder>   新建一个网站。如果没有设置 folder ，Hexo 默认在目前的文件夹建立网站
$ cd <folder>
$ npm install
```
###安装 hexo-deployer-git
  ```  
$ npm install hexo-deployer-git --save
```

##hexo-CentOS
####本次安装环境
CentOS 6.9 i386
####什么是 Hexo？
Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
##安装
安装 Hexo 只需几分钟时间，安装过程遇到问题或无法找到解决方式，请[提交问题](https://github.com/hexojs/hexo/issues)，
安装前提
安装 Hexo 相当简单。然而在安装前，您必须检查电脑中是否已安装下列应用程序：
[Node.js](https://lihuate.github.io/2017/11/04/nodejs-CentOS/)
[Git](https://lihuate.github.io/2017/11/03/githubdeploy/)
####所有必备的应用程序安装完成后，即可使用 npm 安装 Hexo。
``` 
$ npm install -g hexo-cli
```
###安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。
 ```
```$ hexo init <folder>   新建一个网站。如果没有设置 folder ，Hexo 默认在目前的文件夹建立网站
$ cd <folder>
$ npm install
```
###安装 hexo-deployer-git
  ```  
$ npm install hexo-deployer-git --save
```
###修改配置。
_config.yml
网站的配置信息，在_config.yml配置
```
deploy:
  type: git
  repo: <repository url>  库（Repository）地址
  branch: [branch]     分支名称。如果您使用的是 GitHub 或 GitCafe 的话，程序会尝试自动检测。
  message: [message]   自定义提交信息 (默认为 Site updated: {{ now('YYYY-MM-DD HH:mm:ss') }})
```
###安装 hexo-deployer-heroku。
```
$ npm install hexo-deployer-heroku --save
```
修改配置。
```
deploy:
  type: heroku   
  repo: <repository url>   Heroku 库（Repository）地址
  message: [message]  自定提交信息 (默认为 Site updated: {{ now('YYYY-MM-DD HH:mm:ss') }})
  ```
###快速开始
创建一个新的帖子
```
hexo new "My New Post"  空格用引号包住
```
###运行服务器
```
hexo server    hexo s
```
###生成静态文件
```
hexo generate  hexo g  我每次简写都有问题
```
###部署到远程站点
```
hexo deploy   hexo d
```
