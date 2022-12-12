## adb命令

### 安装apk

```shell
adb install -r xxx.apk
```
### 获取模拟器中的文件

```
adb pull /sdcard/xxx.apk ~/Desktop
```

### 复制文件到android系统

```
adb push xxx.apk /sdcard/
```

### 进入模拟器的shell模式

```
adb shell
```

### 查看系统内存状态

```javascript
adb shell cat /proc/meminfo
adb shell dumpsys meminfo com.etone.edmp  //查看应用内存
```



### 卸载apk

```
adb shell
cd data/app
rm apk包
exit
adb uninstall apk包的主包名
adb install -r apk包
```

### 删除系统应用

```
adb shell
mount -o remount /system （重新挂载系统分区，使系统分区重新可写）。
cd system/app
rm *.apk
```

### 发布端口

```
你可以设置任意的端口号，做为主机向模拟器或设备的请求端口。如： 
adb forward tcp:5555 tcp:8000
```

### 查看连接设备

```
adb devices
获取设备的ID和序列号：
adb get-product 
adb get-serialno
```

### 访问[数据库](http://lib.csdn.net/base/mysql)SQLite3

```
adb shell 
sqlite3
```

### 查看版本

```
adb version
启动 adb server 
adb start-server

停止 adb server 
adb kill-server
```

### 列出手机装的所有app的包名

```
列出手机装的所有app的包名
adb shell pm list packages
列出系统应用的所有包名
adb shell pm list packages -s
列出除了系统应用的第三方应用包名
adb shell pm list packages -3
使用 grep 来过滤
adb shell pm list packages | grep qq
```

### 启动与停止应用

```
启动应用
adb shell am start -n com.stormzhang.demo/.ui.SplashActivity
强制停止应用
adb shell am force-stop <packagename>
// 如：adb shell am force-stop cn.androidstar.demo
```

### 查看日志

```
adb logcat
```

### 重启

```
adb reboot
```

### 获取序列号

```
adb get-serialno
```

### 获取 MAC 地址

```
adb shell  cat /sys/class/net/wlan0/address
```

### 查看设备型号

```
adb shell getprop ro.product.model
```

### 查看 Android 系统版本

```
adb shell getprop ro.build.version.release
```

### 查看屏幕分辨率

```
adb shell wm size
```

### 查看屏幕密度

```
adb shell wm density
```

### 截屏

```shell
/system/bin/screencap -p /sdcard/screen.png
```



### 更多命令

```
https://github.com/mzlogin/awesome-adb
```

