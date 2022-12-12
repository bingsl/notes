# centos7安装adb

## 安装java

* 下载安装JDKrpm包我的版本是jdk-8u101-linux-x64.rpm
* 检查旧版本

```
# rpm -aq | grep java
```

* 删除旧版本以上命令输出的就版本如下java-1.7.0-openjdk此为包全名

```
# rpm -e --nodeps java-1.7.0-openjdk
```

* 安装下载好的rpm包

```
# rpm -ivh jdk-8u101-linux-x64.rpm
```

* 配置JDK环境

```
# vim /etc/profile

输入以下内容：
 JAVA_HOME=/usr/java/jdk1.8.0_101
 JRE_HOME=/usr/java/jdk1.8.0_101/jre
 PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin      
 CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
 export JAVA_HOME JRE_HOME PATH CLASSPATH

# source /etc/profile     //使修改生效
```

## 安装配置SDK

* 下载androidSDK

```
cd /opt

mkdir androidSdk

wget https://dl.google.com/android/repository/sdk-tools-linux-3859397.zip

unzip sdk-tools-linux-3859397.zip
```

* 配置命令

打开 /etc/profile添加sdk命令如下

```
export PATH USER LOGNAME MAIL HOSTNAME HISTSIZE HISTCONTROL
export PATH=$PATH:/opt/androidSdk/tools/bin
```

然后执行`source profile` 使之生效

* 查看已安装信息

```
sdkmanager --list

	......
  system-images;a...pis;armeabi-v7a | 6            | Google APIs ARM EABI v7a Syste...
  system-images;a...google_apis;x86 | 6            | Google APIs Intel x86 Atom Sys...
  system-images;a...gle_apis;x86_64 | 6            | Google APIs Intel x86 Atom_64 ...
  system-images;a...;android-tv;x86 | 3            | Android TV Intel x86 Atom Syst...
  system-images;a...ndroid-wear;x86 | 1            | Android Wear Intel x86 Atom Sy...
  system-images;a...google_apis;x86 | 4            | Google APIs Intel x86 Atom Sys...
  system-images;a...s_playstore;x86 | 4            | Google Play Intel x86 Atom Sys...
  tools                             | 26.0.2       | Android SDK Tools                

Available Updates:
  ID      | Installed | Available
  ------- | -------   | -------  
  tools   | 26.0.1    | 26.0.2
```

* 安装需要的package

```
sdkmanager "build-tools;26.0.2"
```

* 配置platform tools

```
platform tools下载地址

http://downloads.puresoftware.org/files/android/platform-tools/
```



打开`/etc/profile`添加如下命令:

```
cd /etc

//添加结果
...
export PATH USER LOGNAME MAIL HOSTNAME HISTSIZE HISTCONTROL
export PATH=$PATH:/opt/androidSdk/tools/bin
export PATH=$PATH:/opt/androidSdk/platform-tools
...

//然后执行更新生效
source profile
```

* 执行adb shell

```
adb shell
-bash: /opt/androidSdk/platform-tools/adb: /lib/ld-linux.so.2: bad ELF interpreter: 没有那个文件或目录

原来是因为系统的问题，因为我的系统是64位的，那adb这个程序是32位的

解决方案：
yum install glibc.i686
yum install libstdc*
yum install libstdc++.so.6
```



* 查看配置结果

```
# adb version
```



## 参考文档

```
http://blog.devwiki.net/index.php/2017/07/20/centos-install-android-sdk.html

http://blog.csdn.net/zggzcgy/article/details/12969255
```

