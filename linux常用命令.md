####Linux

根据本人日常使用的linux命令总结：分享一些常用实用的liunx命令

```powershell
cd pathname  #进入某个文件路径，pathname文件夹名
cd - #回到上次的文件目录
cd #回车，回到家目录
vi file  #file指文件名，vi file 以编辑方式打开文件，按下a键开始编辑，按下esc键,:键，wq键即可保存文件，q!表示不保存强制退出，
pwd  #查看当前工作区
mkdir test #创建文件夹
touch text.txt    #创建文件
cat  //testword.html #查看文件
ll  #查看该目录下所有文件详情包括权限
ls   #查看所有文件目录
ls -a #显示所有文件,包括隐藏文件和目录
ls -l #显示文件，包括权限，在某些没有ll别名的环境，ll==ls -l
date #查看当前系统的时间
cal 月 年 #显示指定年份和月份的日历，如cal 1 2020 会出现2020年一月份的日历
#单独指定年，这时输出全年的日历
```

[![图片](https://dn-coding-net-production-pp.codehub.cn/b6a4359b-1a56-4e2c-8ad1-d5c9280ba24a.png?imageView2/2/w/560/ignore-error/1)](https://dn-coding-net-production-pp.codehub.cn/b6a4359b-1a56-4e2c-8ad1-d5c9280ba24a.png)

```javascript
rm -rf  files    //删除文件或文件夹
du -sh files // 查看文件大小
du -sh * //查看该目录下所有文件的大小
```

> 如何将本地代码上传至远程服务器

```powershell
scp text.zip  name@ip:/path  #将本地文件scp es6.zip 上传到服务器的path目录下
exp: ssh -p 35791 text@123.56.3.168   #-p 端口号
```

> mv 如何修改一个文件名及将一个文件移到另一个目录

```powershell
mv text test   #将文件夹text 改成 test

mv text.txt   test/    #将文件text.txt移到test目录下
```

[![图片](https://dn-coding-net-production-pp.codehub.cn/5f5a6534-e4a0-49f8-a820-eafda276bb72.png?imageView2/2/w/560/ignore-error/1)](https://dn-coding-net-production-pp.codehub.cn/5f5a6534-e4a0-49f8-a820-eafda276bb72.png)

clear 清空当前item终端界面
ctrl+r 搜索历史命令

```shell
tar czvf my.tar  #text将文件打包成my.tar 
tar xzvf my.tar #解压文件包查找被占用的端口
  1. netstat -tln  
  2. netstat -tln | grep 8083  
 netstat -tln #查看端口使用情况，而netstat -tln | grep 8083 则是只查看端口8083的使用情况
 
#2.查看端口属于哪个程序？端口被哪个进程占用
  1. lsof -i :8083  
 
#3.杀掉占用端口的进程
  1. kill -9 进程id  
 ps -ef|grep
#ps命令将某个进程显示出来,grep命令是查找,中间的|是管道命令 是指ps命令与grep同时执行,PS是LINUX下最常用的也是非常强大的进程查看命令
```

```powershell
zip -r xxx.zip xxx  #将一个目录打包
```

