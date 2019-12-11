# nginx安装使用过程

编译环境指令

```javascript
yum install  pcre-devel zlib-devel -y
yum install gcc gcc-c++ -y
```

获取nginx安装包

```
mkdir nginx-src && cd nginx-src
wget http://nginx.org/download/nginx-1.7.3.tar.gz
```

解压包

```
tar xzf nginx-1.7.3.tar.gz
cd nginx-1.7.3
```

编译

```
./configure
make
make install
whereis nginx
nginx: /usr/local/nginx
cd /usr/local/nginx
cd sbin/
./nginx启动
ps aux|grep nginx查看进程
./nginx -s quit停止nginx
./nginx  -s stop
```

在地址栏输入ip，访问不了，打开控制台会报500，后来修改/usr/local/nginx/conf/nginx.conf替换如下内容

```
user www www;
worker_processes 2; #设置值和CPU核心数一致
error_log /usr/local/webserver/nginx/logs/nginx_error.log crit; #日志位置和日志级别
pid /usr/local/webserver/nginx/nginx.pid;
#Specifies the value for maximum file descriptors that can be opened by this process.
worker_rlimit_nofile 65535;
events
{
  use epoll;
  worker_connections 65535;
}
http
{
  include mime.types;
  default_type application/octet-stream;
  log_format main  '$remote_addr - $remote_user [$time_local] "$request" '
               '$status $body_bytes_sent "$http_referer" '
               '"$http_user_agent" $http_x_forwarded_for';
  
#charset gb2312;
     
  server_names_hash_bucket_size 128;
  client_header_buffer_size 32k;
  large_client_header_buffers 4 32k;
  client_max_body_size 8m;
     
  sendfile on;
  tcp_nopush on;
  keepalive_timeout 60;
  tcp_nodelay on;
  fastcgi_connect_timeout 300;
  fastcgi_send_timeout 300;
  fastcgi_read_timeout 300;
  fastcgi_buffer_size 64k;
  fastcgi_buffers 4 64k;
  fastcgi_busy_buffers_size 128k;
  fastcgi_temp_file_write_size 128k;
  gzip on; 
  gzip_min_length 1k;
  gzip_buffers 4 16k;
  gzip_http_version 1.0;
  gzip_comp_level 2;
  gzip_types text/plain application/x-javascript text/css application/xml;
  gzip_vary on;
 
  #limit_zone crawler $binary_remote_addr 10m;
 #下面是server虚拟主机的配置
 server
  {
    listen 80;#监听端口
    server_name localhost;#域名
    index index.html index.htm index.php;
    root /usr/local/webserver/nginx/html;#站点目录
      location ~ .*\.(php|php5)?$
    {
      #fastcgi_pass unix:/tmp/php-cgi.sock;
      fastcgi_pass 127.0.0.1:9000;
      fastcgi_index index.php;
      include fastcgi.conf;
    }
    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|ico)$
    {
      expires 30d;
  # access_log off;
    }
    location ~ .*\.(js|css)?$
    {
      expires 15d;
   # access_log off;
    }
    access_log off;
  }

}
```



之后输入ip依然是internet error

之后使用命令curl http://127.0.0.1发现在centos服务器上可以请求到安装nginx成功的html内容

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```



后来才知道要去阿里云控制台开放80端口

找到快速创建规则（安全组／配置规则／快速创建规则）

勾选常用端口(TCP) SSH(22),HTTP(80)，授权对象：写0.0.0.0/0保存即可

##### 再输入ip地址，打开网页就能看到Welcome to nginx!的字样

如何部署一个web项目，

```
cd /usr/local/nginx/html/ 
将root下的项目文件夹复制一个到该目录下即可，然后打开http://47.94.6.128/es6/index.html

es6是文件目录，index.html是es6下的初始页
```

nginx -t  测试nginx配置是否成功
nginx -s reload  重新载入配置文件

```nginx
cd /usr/local/nginx/conf
建立一个vhost目录 mkdir vhost
然后在nginx.config 中引入vhost目录中的conf
加入该行include 
vhost/*.config

```

服务器注册的二级域名为36dian.top 因此可以在该服务配置不同的conf 

Xxx1.36dian.top 对应项目1

xxx2.36dian.top 对应项目2





