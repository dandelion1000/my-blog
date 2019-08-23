# jenkins搭建

jenkins是一个开源软件项目，是基于java开发的一种持续集成工具，用于监控持续重复的工作，旨在提供一个开放易用的软件平台。

1.安装jenkins

Java 安装

```
yum -y install java-1.8.0-openjdk-devel
```

为了使用 Jenkins 仓库，我们要执行以下命令：

```
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
```

如果您以前从 Jenkins 导入过 key，那么 `rpm --import` 将失败，因为您已经有一个 key。请忽略，继续下面步骤。

#### 安装

接着我们可以使用 `yum` 安装 Jenkins：

```
yum -y install jenkins
```

2.启动 Jenkins

- 启动 Jenkins 并设置为开机启动：

```
systemctl start jenkins.service
chkconfig jenkins on
```

测试访问

Jenkins 默认运行在 8080端口。

3.进入 Jenkins

#### 管理员密码

登入 Jenkins 需要输入管理员密码，按照提示，我们使用以下命令查看初始密码：

```
cat /var/lib/jenkins/secrets/initialAdminPassword
```

#### 定制 Jenkins

我们选择默认的 `install suggested plugins` 来安装插件。

#### 创建用户

请填入相应信息创建用户，然后即可登入 Jenkins 的世界。



Docker 搭建jenkins

Docker CE

 是一个开源的应用级别的虚拟化工具，可以将任何应用包装在"

LXC容器”中运行。

安装 curl 工具

使用APT包管理工具安装cURL：

```
 apt update && apt install -y curl
```

安装 Docker

官方已经给出了适合 Linux 平台的自动安装脚本。因此想要安装 Docker，只需要运行下面的命令：

```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

###### 2.添加 Docker Hub 镜像加速

您可以通过修改 daemon.json 配置文件来使用 Docker Hub 加速服务。

创建 daemon.json 文件

请在 /etc/docker 目录下创建 daemon.json。

```
touch /etc/docker/daemon.json
```

编辑 daemon.json 文件

编辑 /etc/docker/daemon.json，添加镜像服务地址。腾讯云镜像的配置如下：

```
{
    "registry-mirrors": ["https://mirror.ccs.tencentyun.com"]
}
```

重新启动 Docker

```
systemctl daemon-reload
systemctl restart docker
```

安装jenkins

```
获取 Jenkins 镜像
Jenkins 官方镜像主要提供两个版本，分别是 LTS
 版和每周更新
版。本教程使用的是 LTS 版。
docker pull jenkins/jenkins:lts
启动 Jenkins 容器
使用 Docker 在宿主主机启动 Jenkins 容器：
docker run --name jenkins -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home --restart always -d jenkins/jenkins:lts
```



#### docker run 附加参数说明：

| 附加参数  | 本教程所使用的值 | 用途     |
| --------- | ---------------- | -------- |
| --name    | jenkins          | 命名     |
| -p        | 8080:8080        | 端口映射 |
| -v        | jenkins_home     | 数据卷   |
| --restart | always           | 重启策略 |

有关更多配置方法请查看官方 Docker 文档。

访问 Jenkins 实例

在浏览器中打开 [http://111.230.151.217:8080](http://111.230.151.217:8080/) 即可浏览之前启动的 Jenkins 实例。

获取初始登录密码

回到命令行，执行以下命令：

```
docker exec jenkins \
cat /var/jenkins_home/secrets/initialAdminPassword
```

没有git

```
如果你碰巧用Debian或Ubuntu Linux，通过一条sudo apt-get install git就可以直接完成Git的安装，非常简单。

sudo apt-get install git
```

之后配置源码管理会报错

```
Credentials 添加add 所在仓库的用户名和密码，重新拷贝Repository URL即可
```







