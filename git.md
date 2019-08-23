# git

https://git-scm.com/book/zh/v2

####1.介绍

```
Git是一种非常流行的分布式版本控制系统，它和其他版本控制系统的主要差别在于Git只关心文件数据的整体是否发生变化，而大多数版本其他系统只关心文件内容的具体差异，这类系统（CVS，Subversion，Perforce，Bazaar 等等）每次记录有哪些文件作了更新，以及都更新了哪些行的什么内容。Git另一个比较好的地方在于绝大多数操作都可以在本地执行，而每个本地都可以从服务器获取一份完整的仓库代码，而且在没网的时候仍然可以修改和使用大部分命令，在方便的时候再跟服务器进行同步，这样可以更好的实现多人联合编程。
```

#### 2.安装git

> 可以通过软件包或者其它安装程序来安装，或者下载源码编译安装。

##### liunx上安装

> 可以使用发行版包含的基础软件包管理工具来安装

```powershell
  $ sudo yum install git
  
```

> 若基于 Debian 的发行版上，请尝试用 apt-get：

```powershell
  $ sudo apt-get install git
```

更多unix安装方式参考https://git-scm.com/download/linux

##### Mac 上安装

方法有很多种，这里介绍一种，通过homebrew(**软件包的管理器**)安装

```powershell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
#安装Homebrew
```

```powershell
brew install git
#如果已经安装过，可以通过brew upgrade git命令更新版本
```

#### 3.初次运行 Git 前的配置

```console
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

##### 检查配置信息

```powershell
git config --list
=>
user.name=John Doe
user.email=johndoe@example.com
```

#### 4.获取git帮助

若你使用 Git 时需要获取帮助，有三种方法可以找到 Git 命令的使用手册：

```console
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```

![图片](https://dn-coding-net-production-pp.codehub.cn/c1a46617-4f2e-42fa-a78f-d62ad8767362.png)

