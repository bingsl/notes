## 地图引擎安装

###执行安装脚本安装地图服务

* 把emapinstall.tar.gz压缩包放到～目录并解压

```
# mv emapinstall.tar.gz ~
# tar -zxvf emapinstall.tar.gz
```

* 进入install目录执行 ./install.sh

```
# cd emapinstall
# ./install.sh
```

###注册license

执行安装脚本安装完成后注册license

* 生成license.lcr文件， 用于申请license，会得到license.lcs文件

```
# cd ~/emapinstall/map/
# ./lcsr > license.lcr
```

* 生成的license.lcs放到/etc目录

```
mv license.lcs /etc/
```

* 重启supervisor      `supervisorctl restart mycheck`
* 检查license状态  在～/emapinstall目录执行 `./mycheck`  如果结果是`license legal` 则license注册成功。

```
# cd ~/emapinstall/map/
# ./mycheck
```

注：如果需要重新生成license需要重新生成lcr文件 ,在～/install目录执行

```
# cd ~/emapinstall/map/
# ./lcsr > license.lcr
```

###安装完成

地图接口：http://IP:12450/service.php

地图上传地址：http://IP:12450/upload.html

地图编辑器：http://IP:8888    admin/zxcv@1234

### 注意事项

```
1.安装完成检查各项服务是否正常启动
    #netstat -nltp
    查看正在使用的如下端口：
    12450   地图接口
    8888    地图编辑器
    5432    地图数据库
    3306    地图编辑器数据库

2.脚本安装postgresql时，有时会安装失败
    检查postgresql是否安装：
    # psql -V
    检查postgresql是否正常运行
    # systemctl status postgresql-9.5

3. 如果地图接口返回数据异常，没有正常获取到地图相关数据，检查license是否正常
    # cd ~/emapinstall/map/
    # ./mycheck
```

### 常见问题

* license 错误

  * 调用接口返回` license error`
  * 接口返回`cannot connect server`

  ```
  解决方法：
   一般先检查license是否正常  
    # cd ~/emapinstall/map/
    # ./mycheck
    如果发现license异常可以重启supervisor尝试修复 supervisorctl restart mycheck
    若以上步骤不能解决license问题，请重新上传license.lcr 文件到license平台
    
    注意：
      服务器网卡，内存，硬盘变动license会失效
  ```

*  上传错误

  * 上传返回`502 Bad GateWay`

  ```
  解决方法：
    方法一： 重启上传服务
    # kill -9 $(ps -ef|grep backup.py|gawk '$0 !~/grep/ {print $2}' |tr -s '\n' ' ')
    
    # spawn-fcgi -f /var/www/cgi-bin/backup/backup.py -a 127.0.0.1 -p 5679 -F 3 -u nginx
    
    方法二：检查PostgreSQL是否安装或正常运行，服务运行端口5432，可查看5432端口的状态
     # psql -V
     # systemctl status postgresql-9.5
  ```


