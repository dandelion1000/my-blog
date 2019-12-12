# nginx安装使用过程

> 编译环境指令

```shell
yum install  pcre-devel zlib-devel -y
yum install gcc gcc-c++ -y
```

> 获取nginx安装包

```shell
mkdir nginx-src && cd nginx-src
wget http://nginx.org/download/nginx-1.7.3.tar.gz
```

> 解压包

```shell
tar xzf nginx-1.7.3.tar.gz
cd nginx-1.7.3
```

> 编译

```shell
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

nginx -t  测试nginx配置是否成功
nginx -s reload  重新载入配置文件

```shell
cd /usr/local/nginx/conf
#建立一个vhost目录 
mkdir vhost
#然后在nginx.config 中引入vhost目录中的conf,加入如下内容include 
vhost/*.config

```

> 以服务器注册的二级域名为36dian.top为例， 因此可以在该服务配置不同的conf 以对应不同站点

Xxx1.36dian.top.conf 对应项目1

xxx2.36dian.top.conf 对应项目2

最后去目录/usr/local/nginx/sbin下重启nginx      ./nginx -s reload

```shell
为避免每次修改/配置完conf后都需要去对应目录重启，我们可以简化命令
打开根目录下.bashrc 文件,加入如下一行
alias nginx reload ='/usr/local/nginx/sbin/nginx -s reload'
:wq保存退出后输入命令
source .bashrc
接下来我们在任何地方都可以使用该命令 nginx reload进行nginx重启了
```



> 附上nginx.config

```nginx
events
{
  use epoll;
  worker_connections 65535;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    server {
      listen 80;
      server_name www.36dian.top; #域名
      index index.html index.htm index.php;
      root /usr/local/nginx/html; #默认域名页面
    }
    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    include vhost/*.conf;    #引入配置多站点conf
}
```

> 附上vhost/utime.36dian.top.conf 简单配置

```nginx
server {
 listen 80;
 server_name utime.36dian.top;
 root /home/wwwroot/fesite/humor/dist/;
 location / {
   index index.html index.htm index.php;
 }
}
```

> 附在第一次购买服务器安装nginx时遇到的问题：
>
> 安装完nginx后，在地址栏输入ip，访问不了，打开控制台会报500，后来修改/usr/local/nginx/conf/nginx.conf替换如下内容

```nginx
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
 
  #limit_zone crawler $binary_remote_addr 10m;
 #下面是server虚拟主机的配置
 server
  {
    listen 80;#监听端口
    server_name localhost;#域名
    index index.html index.htm index.php;
    root /usr/local/webserver/nginx/html;#站点目录 
    #在不配置其他conf的情况下如果需要部署静态页面，只需将html文件目录拷贝到这里即可，以部署es6文档html为例，拷贝es6文件到该目录下，然后打开http://{ipname}/es6/index.html就可以访问了，index.html是es6下的初始页
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

```html
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