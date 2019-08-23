#####nginx配置文件说明

nginx 静态资源的加载，反向代理，重写，和负载分担。

静态资源的代理，主要做图片加载，app包下载等功能。在进行nginx配置静态资源加载中，我主要是在server{}模块中进行location{}模块的匹配配置

######Location 常用配置详解

> 语法规则：location [=|~|~*|^~]/url/{...}

= 开头表示精确匹配

^~ 开头表示url以某个常规字符串开头，理解为匹配url路径。nginx不对url做编码，因此请求为/static/20%/aa，可以被规则^~ /static/ /aa匹配到（注意是空格）

～ 开头表示区分大小写的正则匹配

～* 开头表示不区分大小写的正则匹配

!~和!~* 分别为区分大小写不匹配及不区分大小写不匹配 的正则

/   通用匹配，任何请求都会匹配到。

#####1.Nginx 指定文件路径

Nginx 指定文件路径有两种方式root和alias。指令的使用方法和作用域：

###### 1.root

> 根路径配置，用于访问文件系统，在匹配到location配置的URL路径后，指向【root】配置的路径，并把location配置路径附加到其后

```nginx
location ^~ /static {
	root /home/bae/wwwroot/agency-h5/;
}
```

语法：root path

默认值：root html

配置段：http ,server.location,if

###### 2.alias:----别名配置，用于访问文件系统，在匹配到location配置的url路径后，指向alias配置的路径

语法：alias  path

配置段：location

```nginx
location ^~ /static2/ {
  alias /home/bae/wwwroot/agency-h5-test/dist/static/;
 }
//请求／static2/下文件，将会返回文件/home/bae/wwwroot/agency-h5-test/dist/static下的文件
```

root与alias主要区别在于nginx如何解释location后面的url，这会使两者分别以不同的方式将请求映射到服务器文件上。

**root的处理结果是 root路径＋location路径**

**alias的处理结果是：使用alias路径替换location路径**

alias是一个目录别名的定义，root则是最上层目录的定义。
还有一个重要的区别是alias后面必须要用“/”结束，否则会找不到文件的。。。而root则可有可无~~

如果一个请求的URI是/t/a.html时，web服务器将会返回服务器上的/www/root/html/new_t/a.html的文件。注意这里是new_t，因为alias会把location后面配置的路径丢弃掉，把当前匹配到的目录指向到指定的目录。

注意：
\1. 使用alias时，目录名后面一定要加"/"。
\3. alias在使用正则匹配时，必须捕捉要匹配的内容并在指定的内容处使用。
\4. alias只能位于location块中。（root可以不放在location中）

#####2.include

> 语法：include file|*

```powershell
#你可以包含一些其他的配置文件来完成你想要的功能。0.4.4版本以后，include指令已经能够支持文件通配符：
eg:include vhosts/*.conf;
```

##### 3.try_files

> 语法：try_files path1 [ path2] uri 

```
按照顺序检查存在的文件，并且返回找到的第一个文件，斜线指目录：$uri / 。如果在没有找到文件的情况下，会启用一个参数为last的内部重定向，这个last参数“必须”被设置用来返回URL，否则会产生一个内部错误。
location / {
  try_files /system/maintenance.html
  $uri $uri/index.html $uri.html @mongrel;
}
 
location @mongrel {
  proxy_pass http://mongrel;
```

在这个例子中，这个try_files指令：

```
location / {
try_files $uri $uri/ @drupal;
}
等同于下列配置：
location / {
error_page     404 = @drupal;
log_not_found  off;
}
```



```
location / {
  try_files index.html index.htm @fallback;
}

location @fallback {
  root /var/www/error;
  index index.html;
}
```



##### 4.error_page

> 语法:error_page code [ code... ] [ = | =answer-code ] uri | @named_location 

```
error_page   404          /404.html;
error_page   502 503 504  /50x.html;
error_page   403          http://example.com/forbidden.html;
error_page   404          = @fetch;
```

如果在重定向时不需要改变URI，可以将错误页面重定向到一个命名的location字段中：

```
location / (
    error_page 404 = @fallback;
)

location @fallback (
    proxy_pass http://backend;
```



##### 5.proxy_pass--反向代理配置，用于代理请求适用于前后端负载分离或多台机器、服务器负载分离的场景，在匹配到location配置的URL路径后，转发请求到【proxy_pass】配置的URL，是否会附加location配置路径与【proxy_pass】配置的路径后是否有"/"有关，有"/"则不附加，如：

```nginx
location /test/ { 
    proxy_pass http://127.0.0.1:8080/; 
}
//请求/test/1.jpg（省略了协议与域名），将会被nginx转发请求到http://127.0.0.1:8080/1.jpg（未附加/test/路径）。
```

##### 6.重写

Rewrite 重写是将请求的url进行重新修改。要支持重新功能，必须在安装编译nginx时进行加载PCRE库，这样就可以进行使用了

```nginx
location = / {
  rewrite ".*" http://192.168.1.1/index.jsp    
 }
//将访问后缀等于 /   的请求，直接重新到http://192.168.1.1/index.jsp 
```



 if判断配合变量的使用。 在工作中有时需要根据变量进行分发，或者对变量进行重新定义等操作。 

```nginx
eg:  下面2个URL，需要通过参数变量值进行从新分发，然后传递给192.168.1.3的tomcat
http://192.168.1.2/test?testname=test11 -->http://192.168.1.3/test11/22
http://192.168.1.2/test?testname=test12 -->http://192.168.1.3/test11/33
那么就要使用到if 判断 和参数匹配了
location ~ /test {
if ($arg_testname = "test11") {
    rewrite  ^/(.*)$  http://192.168.1.3/test11/22 break ;
}
if ($arg_testname = "test12") {
    rewrite  ^/(.*)$  http://192.168.1.3/test11/33 break ;
}
}
```



#######更多关于nginx知识

http://manual.51yip.com/nginx/

http://www.nginx.cn/doc/



关于nginx配置crm跨域

```
        add_header 'Access-Control-Allow-Origin' "$http_origin" always;
        add_header 'Access-Control-Allow-Credentials' 'true' always;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS' always;
        add_header 'Access-Control-Allow-Headers' 'Accept,Authorization,Cache-Control,Content-Type,DNT,If-Modified-Since,Keep-Alive,Origin,User-Agent,X-Requested-With' always;
```

