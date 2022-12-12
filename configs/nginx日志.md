

   在http模块添加

```
log_format access_json  '{"@timestamp":"$time_iso8601",'
                           '"host":"$server_addr",'
                           '"clientip":"$remote_addr",'
                           '"size":$body_bytes_sent,'
                           '"responsetime":$request_time,'
                           '"upstreamtime":"$upstream_response_time",'
                           '"upstreamhost":"$upstream_addr",'
                           '"http_host":"$host",'
                           '"url":"$uri",'
                           '"domain":"$host",'
                           '"xff":"$http_x_forwarded_for",'
                           '"referer":"$http_referer",'
                           '"status":"$status"}';
access_log  /var/log/nginx/access.log  access_json;


```

```
log_format name(格式名称) type(格式样式)  
```



```
log_format access_json escape=json '{"@timestamp":"$time_iso8601",'
                           '"server_addr":"$server_addr",'
                           '"request":"$request",'
                           '"query_string":"$query_string",'
                           '"request_body":"$request_body",'
                           '"http_accept_language":"$http_accept_language",'
                           '"http_user_agent":"$http_user_agent",'
                           '"remote_addr":"$remote_addr",'
                           '"body_bytes_sent,":$body_bytes_sent,'
                           '"request_time,":$request_time,'
                           '"request_length":$request_length,'
                           '"http_host":"$http_host",'
                           '"url":"$uri",'
                           '"host":"$host",'
                           '"http_x_forwarded_for":"$http_x_forwarded_for",'
                           '"http_referer":"$http_referer",'
                           '"status":"$status"}';

```

比如：

```
{
"@timestamp":"2022-12-08T14:02:33+00:00",
"server_addr":"127.0.0.1",
"request":"GET /index.html HTTP/1.1",
"http_accept_language":"zh-CN,en-US;q=0.7,en;q=0.3",
"http_user_agent":"Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:69.0) Gecko/20100101 Firefox/69.0",
"remote_addr":"127.0.0.1",
"body_bytes_sent,":0,
"request_time,":0.000,
"request_length":448,
"http_host":"localhost",
"url":"/index.html",
"host":"localhost",
"http_x_forwarded_for":"-",
"http_referer":"-",
"status":"304"
}

```

参数含义

```
1.访问时间 ts
2.访问端口 server_addr
3.请求方式（GET或者POST等）request
4.用户浏览器语言。如：上例中的 "es-ES,es;q=0.8" http_accept_language
5.用户浏览器其他信息，浏览器版本、浏览器类型等 http_user_agent
6.客户端（用户）IP地址 remote_addr
7.发送给客户端的文件主体内容的大小 body_bytes_sent
8.整个请求的总时间 request_time
9.请求的长度（包括请求行，请求头和请求正文）request_length
10.请求的url地址（目标url地址）的host http_host
11.请求url地址（去除host部分） uri
12.host 与 http_host的区别在于当使用非80/443端口的时候,http_host = host:port host
13.客户端的真实ip，通常web服务器放在反向代理的后面，这样就不能获取到客户的IP地址了，通 过$remote_add拿到的IP地址是反向代理服务器的iP地址。反向代理服务器在转发请求的http头信息中，可以增加 x_forwarded_for信息，用以记录原有客户端的IP地址和原来客户端的请求的服务器地址  http_x_forwarded_for
14.记录从哪个页面链接访问过来的（请求头Referer的内容）http_referer
15.请求状态（状态码，200表示成功)  status
```

```
log_format      main    '[$time_local]|$request_time|$status|$body_bytes_sent|$remote_addr|"$request"|"$http_referer"|"$http_user_agent"|$http_x_forwarded_for|$upstream_cache_status|$upstream_response_time|$upstream_status|$upstream_addr'; 
```



- time_local: 访问的时间与时区，比如18/Jul/2012:17:00:01 +0800，时间信息最后的"+0800"表示服务器所处时区位于UTC之后的8小时
- $request_time：整个请求的总时间，以秒为单位
- $status：记录请求返回的http状态码，比如成功是200。
- $uptream_status：upstream状态，比如成功是200.
- $body_bytes_sent：发送给客户端的文件主体内容的大小，比如899，可以将日志每条记录中的这个值累加起来以粗略估计服务器吞吐量
- $remote_addr：远程客户端的IP地址。
- $request：请求的URI和HTTP协议，这是整个PV日志记录中最有用的信息，记录服务器收到一个什么样的请求
- $http_referer：记录从哪个页面链接访问过来的（请求头Referer的内容 ）
- $http_user_agent：客户端浏览器信息（请求头User-Agent的内容 ）
- $http_x_forwarded_for：客户端的真实ip，通常web服务器放在反向代理的后面，这样就不能获取到客户的IP地址了，通 过$remote_add拿到的IP地址是反向代理服务器的iP地址。反向代理服务器在转发请求的http头信息中，可以增加 x_forwarded_for信息，用以记录原有客户端的IP地址和原来客户端的请求的服务器地址。
- $upstream_cache_status

​     *MISS*
​     *EXPIRED - expired, request was passed to backend*
​     *UPDATING - expired, stale response was used due to proxy/fastcgi_cache_use_stale updating*
​     *STALE - expired, stale response was used due to proxy/fastcgi_cache_use_stale*
​     *HIT - (dash) - request never reached to upstream module. Most likely it was processed at Nginx-level only (e.g. forbidden, redirects, etc) (Ref: Mail Thread)*

- $upstream_response_time 请求过程中，upstream的响应时间,以秒为单位
- $uptream_status：upstream状态，比如成功是200.
- $upstream_addr:upstream的地址，即真正提供服务的主机地址
- $remote_user：远程客户端用户名称，用于记录浏览者进行身份验证时提供的名字，如登录百度的用户名scq2099yt，如果没有登录就是空白。

```
access_log      /usr/local/nginx/access.log  main;  
```



日志按天分割

```
log_format access_json escape=json '{"@timestamp":"$time_iso8601",'
                           '"server_addr":"$server_addr",'
                           '"request":"$request",'
                           '"query_string":"$query_string",'
                           '"request_body":"$request_body",'
                           '"http_accept_language":"$http_accept_language",'
                           '"http_user_agent":"$http_user_agent",'
                           '"remote_addr":"$remote_addr",'
                           '"body_bytes_sent,":$body_bytes_sent,'
                           '"request_time,":$request_time,'
                           '"request_length":$request_length,'
                           '"http_host":"$http_host",'
                           '"url":"$uri",'
                           '"host":"$host",'
                           '"http_x_forwarded_for":"$http_x_forwarded_for",'
                           '"http_referer":"$http_referer",'
                           '"status":"$status"}';

        log_format  main  '$remote_addr - $remote_user [$time_iso8601] "$request" '
                      '$status $body_bytes_sent $request_body  "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
        map $time_iso8601 $logdate {
           '~^(?<ymd>\d{4}-\d{2}-\d{2})' $ymd;
           default 'nodate';
        }
        access_log  /usr/local/nginx/logs/access.log main;
        access_log  /usr/local/nginx/logs/access-$logdate.log main;
```



nginx日志中$request_body 十六进制字符(\\x22) 引号问题处理记录

**改成**
**重点：** `log_format json_log escape=json`

```
log_format json_log escape=json '{"realip":"$remote_addr","@timestamp":"$time_iso8601","host":"$http_host","request":"$request","req_body":"$request_body","status":"$status","size":$body_bytes_sent,"ua":"$http_user_agent","cookie":"$http_cookie","req_time":"$request_time","uri":"$uri","referer":"$http_referer","xff":"$http_x_forwarded_for","ups_status":"$upstream_status","ups_addr":"$upstream_addr","ups_time":"$upstream_response_time"}';

官方文档ngx_http_log_module.html#log_format
注意， escape
是从 1.11.8
后新增的参数
```



参考链接：

```
https://blog.csdn.net/echizao1839/article/details/80872378
```



分割Nginx的access.log日志并保留30天一个月时长，自动删除多余的日志

```
https://blog.csdn.net/qq_30320541/article/details/126932425

https://blog.csdn.net/liuxl57805678/article/details/126893491
```

