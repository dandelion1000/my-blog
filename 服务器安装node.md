```shell
wget https://nodejs.org/dist/v10.14.2/node-v10.14.2-linux-x64.tar.xz #获取node压缩包
tar -xvJf node-v10.14.2-linux-x64.tar.xz #解压
mv node-v10.14.2-linux-x64 /usr/local/nodejs #移动重命名
vim /etc/profile  找到最后一行加入以下内容
export PATH=${PATH}:/usr/local/nodejs/bin/ #添加环境变量
 source /etc/profile   #刷新权限
node  -v  #检测版本，判断安装成功

```