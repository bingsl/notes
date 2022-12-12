



### Vultr官网

https://www.vultr.com/

1598092989@qq.com/Aa1598092989



更改root密码

```
passwd root newpasswd
```



### 安装

* 下载

```
wget –no-check-certificate  https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh
```

* 执行安装脚本

```
#chmod +x shadowsocks.sh

//centos8会报错libsodium install failed! 需要安装Development Tools
#yum -y groupinstall "Development Tools"  

#./shadowsocks.sh 2>&1 | tee shadowsocks.log
```

* 配置

```shell
出现下图后输入SS密码-确认SS密码-输入SS端口-确认SS端口

1 设置密码
2 设置端口
3 设置加密方式 

[root@bing ~]# ./shadowsocks.sh 2>&1 | tee shadowsocks.log


#############################################################
# One click Install Shadowsocks-Python server               #
# Intro: https://teddysun.com/342.html                      #
# Author: Teddysun <i@teddysun.com>                         #
# Github: https://github.com/shadowsocks/shadowsocks        #
#############################################################

Please enter password for shadowsocks-python
(Default password: teddysun.com):pheicloud

---------------------------
password = pheicloud
---------------------------

Please enter a port for shadowsocks-python [1-65535]
(Default port: 14363):

---------------------------
port = 14363
---------------------------

Please select stream cipher for shadowsocks-python:
1) aes-256-gcm
2) aes-192-gcm
3) aes-128-gcm
4) aes-256-ctr
5) aes-192-ctr
6) aes-128-ctr
7) aes-256-cfb   //选这个加密方式
8) aes-192-cfb
9) aes-128-cfb
10) camellia-128-cfb
11) camellia-192-cfb
12) camellia-256-cfb
13) chacha20-ietf-poly1305
14) chacha20-ietf
15) chacha20
16) rc4-md5
Which cipher you'd select(Default: aes-256-gcm):7

---------------------------
cipher = aes-256-cfb
---------------------------


Press any key to start...or Press Ctrl+C to cancel

```

* 安装完成

```shell
Congratulations, Shadowsocks-python server install completed!
Your Server IP        :  198.13.53.219 
Your Server Port      :  14363 
Your Password         :  pheicloud 
Your Encryption Method:  aes-256-cfb 

Welcome to visit:https://teddysun.com/342.html
Enjoy it!

```

