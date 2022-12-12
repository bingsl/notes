# 使用 Certbot 自动申请并续订阿里云 DNS 免费泛域名证书



首先安装 Python 3

```
yum install -y python39
```



创建并激活虚拟环境

```
mkdir -p /mnt/certbot
cd /mnt/certbot
python3 -m venv venv
source venv/bin/activate
```

安装 Certbot 

```
pip install certbot certbot-nginx
```



若nginx未安装在默认路径(/etc/nginx or /usr/local/etc/nginx)下需自己指定nginx路径，到conf目录

nginx 创建软连接

```
ln -s /usr/local/nginx/sbin/nginx /usr/bin/nginx
ln -s /usr/local/nginx/conf/* /etc/nginx
```



手动生成证书

```
certbot certonly -d *.et.xxx.top --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory
```



提示错误时可以手动在阿里云上添加TXT记录

假如出错显示

```python
certbot.errors.AuthorizationError: Some challenges have failed.
```

这就得按照提示来去进行txt验证

```shell
-------------------------------------------------------------------------------
Please deploy a DNS TXT record under the name
_acme-challenge.YOURDOMAIN with the following value:

Fhx3AXM****************e4TchYU

Once this is deployed,
-------------------------------------------------------------------------------
Press Enter to Continue
```

此时登录域名管理后台,添加`_acme-challenge.YOURDOMAIN`域名的TXT记录,值为`Fhx3AXM****************e4TchYU`,保存后输入以下命令进行确认已经正常解析:
`dig -t txt _acme-challenge.YOURDOMAIN`
如果返回结果中有上面填写的值说明已经添加并解析成功,此时返回证书更新界面按回车继续.正常情况下会返回如下结果.

```shell
Waiting for verification...
Cleaning up challenges
Generating key (2048 bits): /etc/letsencrypt/keys/0001_key-certbot.pem
Creating CSR: /etc/letsencrypt/csr/0001_csr-certbot.pem
```



生成证书存放的目录

```
Certificate is saved at: /etc/letsencrypt/live/et.xxx.top/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/et.xxx.top/privkey.pem
```

续期

```
certbot renew
```





两个文件合并成一个并命名为`_.et.xxx.top.pem`

```
fullchain.pem
privkey.pem
```

nginx配置

```
				server_name yue.et.xxx.top;
        ssl_certificate /etc/nginx/conf.d/cert/_.et.xxx.top.pem;
        ssl_certificate_key /etc/nginx/conf.d/cert/_.et.xxx.top.pem;
        include ssl.conf;
```



阿里云添加用户获取AccessKey

在阿里云 https://ram.console.aliyun.com/ 创建一个子账号并配置 RAM 权限 **AliyunDNSFullAccess**，并为子账号生成AccessKey用于通过API管理DNS解析。

```
https://ram.console.aliyun.com/users
```





  参考

```
https://www.cnblogs.com/zhenyauntg/p/15672063.html

http://t.zoukankan.com/jinjiyese153-p-13396918.html
```





### 单一域名

```
# 主要参数:
# --webroot 方式只适用于指定域名的情况, --manuel 手动方式,适用于泛解析的情况, 例如 a.xx.com b.xx.com ....等更多可能的域名
# 参数说明: 
# -w 配合 --webroot出现,定义certbot根据请求生成验证文件的目录
certbot certonly --webroot -d *.domain.com --email xx@xx.com -w /var/www/_letsencrypt -n --agree-tos --force-renewal

```

### 泛解析(人工配置 DNS TXT方式)

```
#本次案例使用 manuel方式申请证书, 指定challenges为 dns-01方式, 只有此方式能完成泛解析,否则chrome等浏览器会报请求不安全
#若非要使用第一种方式,可以在chrome或edge页面中输入thisisunsafe加载页面,不推荐
certbot certonly --manual -d *.zheng.binginfo.top --email 1598092989@qq.com  --preferred-challenges dns-01 --force-renewal --server https://acme-v02.api.letsencrypt.org/directory

```

```
#terminal中说明:
# 需要先配置 DNS TXT 
# 具体为一个 _acme-challenge.domain.com 的文件中填入以下内容xxxxx
Please deploy a DNS TXT record under the name
-acme-challenge.domain.com with the following value:

xxxxxxxxxxxxx

Before continuing, verify the record is deployed.


根据提示内容配置DNS TXT,切勿在未配置相应的DNS TXT记录的情况下下一步


```

```
#新开一个session
#本例是阿里云服务器, 后台直接可以修改配置,不多赘述
#使用以下命令检查配置是否生效
dig  -t txt  _acme-challenge.domain.com @8.8.8.8
#返回中包含对应配置部分则说明配置生效,下文600为配置的ttl,视情况而定

#;; ANSWER SECTION:
#_acme-challenge.domain.com. 600 IN	TXT	"xxxxxxxxxxxx"

```

```
配置成功后回车, 几秒后会提示证书的位置: 本例为: /etc/letsencrypt/live/domain.com/xxxx.pem`
`可以使用certbot delete --cert-name domain.com删除此前配置的证书
```

### 泛解析域名(脚本方式)

```
该脚本项目可以在续期时使用

yum install git
git clone https://github.com/ywdblog/certbot-letencrypt-wildcardcertificates-alydns-au
cd certbot-letencrypt-wildcardcertificates-alydns-au && chmod 0777 au.sh

#一定查看readme 

vim au.sh
#填写服务器 accesskey 和 sercet

#保存后测试, 服务器有php环境用php, 有python环境用python, 替换命令中关键字即可
#au.sh三个参数: 执行脚本的环境:php/python | 云服务器:aly/hwy/txy等 | 对应--manual-xx-hook参数 固定的 add/clean
certbot certonly -d *.xxxxx.com --manual --preferred-challenges dns --dry-run  --manual-auth-hook "/脚本目录/au.sh python aly add" --manual-cleanup-hook "/脚本目录/au.sh python aly clean"


#测试无误,去除--dry-run再次执行,获取证书
certbot certonly -d *.xxxxx.com --manual --preferred-challenges dns --manual-auth-hook "/脚本目录/au.sh python aly add" --manual-cleanup-hook "/脚本目录/au.sh python aly clean"

```

证书续期
记录: 2022-06-15
对于单一域名的证书而言, 使用 certbot renew 命令就可以自动完成续期, 但对于泛解析域名, 在使用该命令续期时提示手动脚本是缺少的,这里就是个坑了, 因为使用人工配置DNS TXT的方式申请泛域名证书, 在续期时certbot无法完成人工的那个部分导致续期失败; 但certbot提供了脚本来调用服务商接口更新DNS TXT信息, 但官方的脚本都是针对国外服务器厂商的, 国内需要使用

> https://github.com/ywdblog/certbot-letencrypt-wildcardcertificates-alydns-au



#### 此方式只能对单一域名证书做到自动续期



```
#certbot 续期命令可能已经自动添加到系统的定时任务里了, 可以检查相应位置
crontab -l
/etc/cron.*/* 
systemctl list-timers

#本次实例中未自动添加,手动增加一个定时任务即可
#脚本
#说明 --renew-hook 执行脚本, 至于执行什么完全自定义
certbot renew --force-renewal --renew-hook "openresty -s reload" > /xxxxxx/ssl-renew/ssl-update.log 2>&1 &
#其中cron表达式自己随意定义
crontab -e
#自行添加

```

#### 泛解析域名的自动续期

```
#有git忽略此步骤
yum install -y git
#获取项目
git clone https://github.com/ywdblog/certbot-letencrypt-wildcardcertificates-alydns-au
cd certbot-letencrypt-wildcardcertificates-alydns-au
#提供了php脚本和python脚本
#查看项目中的README,基本上就已经解释的很清楚了,这里只是简述一下
chmod 0777 au.sh

```

**编辑一下脚本填写配置**

```
vim au.sh
#填写阿里云的AccessKey ID及AccessKey Secret
#如何申请见https://help.aliyun.com/knowledge_detail/38738.html
ALY_KEY=""
ALY_TOKEN=""
#填写腾讯云的SecretId及SecretKey
#如何申请见https://console.cloud.tencent.com/cam/capi
TXY_KEY=""
TXY_TOKEN=""
#填写华为云的 Access Key Id 及 Secret Access Key
#如何申请见https://support.huaweicloud.com/devg-apisign/api-sign-provide.html
HWY_KEY=""
HWY_TOKEN=""
#PHP 命令行路径，如果有需要可以修改
phpcmd="/usr/bin/php"
#Python 命令行路径，如果有需要可以修改
pythoncmd="/usr/bin/python"

```

**测试**
`续期时需要使用--manual-clean-hook清除信息, --manual-auth-hook配置信息`
`au.sh 三个参数: [php/python] [aly/hwy等] [对应hook为 add/clean]`

```
#建议看看 certbot -h
# --dry-run参数在申请和续期时都可以用,等于 “测试”
#因为服务器上有php环境,所以参数填写php
certbot renew --dry-run --manual --preferred-challenges dns --manual-auth-hook "/脚本全路径/au.sh python aly add" --manual-cleanup-hook "/脚本全路径/au.sh python aly clean"
#如果使用python环境,则把 php 替换为 python 即可

#最后将输出
Congratulations, all simulated renewals succeeded: 
  /etc/letsencrypt/live/xxxxxx/fullchain.pem (success)
#也就续期成功了

```

**实际续期**
全部证书

```
./certbot renew --manual --preferred-challenges dns --manual-auth-hook "/脚本全路径/au.sh python aly add" --manual-cleanup-hook "/脚本全路径/au.sh python aly clean"

```

指定证书

```
certbot certificates #查看所有证书
	#Found the following certs:
  	#Certificate Name: 证书名称
    #Serial Number: xxxxxxx
    #Key Type: RSA
    #Domains: *.xxxx.com
    #Expiry Date: 2022-09-29 01:23:03+00:00 (VALID: 89 days)
    #Certificate Path: /etc/letsencrypt/live/xxxx/fullchain.pem
    #Private Key Path: /etc/letsencrypt/live/xxxx/privkey.pem
certbot renew --cert-name [证书名称]  --manual-auth-hook "/脚本全路径/au.sh python aly add" --manual-cleanup-hook "/脚本全路径/au.sh python aly clean"

```

**crontab**
`个人建议把续期的命令写在一个脚本里, 在用crontab一个表达式调用`

```
#renew.sh里就是上面的certbot renew那些东西了
crontab -e
0 0 1,15 * * root /xxxx/ssl-renew/renew.sh > /dev/null 2>&1 &

```

参考

```
https://blog.csdn.net/weixin_43558927/article/details/123046649
```

