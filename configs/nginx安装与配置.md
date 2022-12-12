* 下载并安装nginx

```she

```

* 查看nginx服务状态

```shell
systemctl status nginx.service
```

```shell
# systemctl start nginx.service       /启动nginx服务
# systemctl restart nginx.service     /重新启动
# systemctl stop nginx.service        /停止服务
# systemctl enable nginx.service      /开机启动
# systemctl disable nginx.service     /禁止开机启动
```

* 安装nginx php-fpm

```shell
rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
yum -y install nginx  

或
wget  nginx.org/packages/mainline/centos/7/x8664/RPMS/nginx-1.9.9-1.el7.ngx.x8664.rpm
rpm -ivh nginx-1.9.9-1.el7.ngx.x86_64.rpm
yum install nginx



yum -y php-fpm  
service php-fpm restart  
service nginx restart  
chkconfig php-fpm on  
chkconfig nginx on  


server {  
    listen       80;  
    server_name  localhost;  
    autoindex    on;  
    #charset koi8-r;  
    #access_log  /var/log/nginx/log/host.access.log  main;  
  
    location / {  
        root   /var/www/html;  
        index  index.html index.htm index.php;  
    }  
  
    location ~ \.php$ {  
        root           /var/www/html;  
        fastcgi_pass   127.0.0.1:9000;  
        fastcgi_index  index.php;  
        fastcgi_param  SCRIPT_FILENAME  /var/www/html$fastcgi_script_name;  
        include        fastcgi_params;  
    }....  
```



* 配置

```shell
默认的配置文件在 /etc/nginx 路径下，使用该配置已经可以正确地运行nginx；
如需要自定义，修改其下的 nginx.conf 或conf.d/default.conf等文件即可.

在修改配置后可以通过使用./nginx -t来检查配置文件是否正确，
使用./nginx -s reload 或是kill -s  SHGHUP <nginx master pid>让nginx在不停止服务的时候，重新读取配置文件并生效

```

```shell
要让配置生效，我们不必重启 nginx 只需要 reload 配置即可。
sudo service nginx reload
```



### 编译安装

```
首先由于nginx的一些模块依赖一些lib库，所以在安装nginx之前，必须先安装这些lib库，这些依赖库主要有g++、gcc、openssl-devel、pcre-devel和zlib-devel 所以执行如下命令安装
```

```
$   yum install gcc-c++  
$   yum install pcre pcre-devel  
$   yum install zlib zlib-devel  
$   yum install openssl openssl--devel 
```

* 安装之前，最好检查一下是否已经安装有nginx

```
 find -name nginx 
 如果系统已经安装了nginx，那么就先卸载
 yum remove nginx 
```

* 首先进入/usr/local目录

```
cd /usr/local
```

* 从官网下载最新版的nginx

```
wget http://nginx.org/download/nginx-1.7.4.tar.gz  
解压nginx压缩包
tar -zxvf nginx-1.7.4.tar.gz  
会产生一个nginx-1.7.4 目录，这时进入nginx-1.7.4目录
cd  nginx-1.7.4  
```

* 接下来安装，使用--prefix参数指定nginx安装的目录,make、make install安装

```
./configure  $默认安装在/usr/local/nginx   
 make  
 make install    
 如果没有报错，顺利完成后，最好看一下nginx的安装目录
 whereis nginx  
```

* 启动停止nginx服务

```
启动：nginx
停止：nginx -s stop
重新载入配置文件:nginx -s reload
```

