## nodejs 安装

### nodejs安装与配置

```shell
1. 下载 node-v6.11.3-linux-x64.tar.xz
2. tar -xv -f node-v6.11.3-linux-x64.tar.xz
3. mv node-v6.11.3-linux-x64 /usr/local/nodejs
```

### 注册环境变量

在系统中注册node有两方式,推荐使用环境变量的方式.

1.注册环境变量(推荐此种方式,以后安装cnpm的时候会自动把多个快捷方式方到/usr/local/node-v6.5.0-linux-x64/bin/下,软链接的方式就太麻烦了)

```shell
vi /etc/profile
在export PATH的上一行添加如下内容
PATH=$PATH:/usr/local/nodejs/bin/
编辑完成后按Esc键 然后输入 :wq 按回车保存退出。
执行source /etc/profile 可以是变量生效，
然后执行 echo $PATH ，看看输出内容是否包含自己添加的内容
```

2.软链接:

```shell
ln -s /usr/local/nodejs/bin/node /usr/local/bin/node
ln -s /usr/local/nodejs/bin/npm /usr/local/bin/npm
```

### 安装uglify-js

```shell
npm install uglify-js -g
```





```javascript
node有一个模块叫n（这名字可够短的。。。），是专门用来管理node.js的版本的。
首先安装n模块：
npm install -g n
第二步：
升级node.js到最新稳定版
n stable
是不是很简单？！
n后面也可以跟随版本号比如：
n v0.10.26
或
n 0.10.26

就这么简单。
另外分享几个npm的常用命令

 npm -v          #显示版本，检查npm 是否正确安装。  
 npm install express   #安装express模块  
 npm install -g express  #全局安装express模块  
 npm list         #列出已安装模块  
 npm show express     #显示模块详情  
 npm update        #升级当前目录下的项目的所有模块  
 npm update express    #升级当前目录下的项目的指定模块  
 npm update -g express  #升级全局安装的express模块  
 npm uninstall express  #删除指定的模块
```

