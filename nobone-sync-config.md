#####Nobone-sync

nobone-sync 是一个远程文件同步开发工具，可以在本地开发远程同步到服务器。

#### 安装

###### 全局安装

```nginx
npm install -g nobone-sync
```

###### 作为依赖包安装

```nginx
npm install nobone-sync
```

#### 使用

###### 远程启动

```
nobone-sync -s
```

###### 本地启动

```javascript
nobone-sync config.js
//config.js是本地配置nobone-sync的js文件路径
```

#### 配置config.js

```javascript
module.exports = {
    localDir: 'localDir',
    remoteDir: 'remoteDir', //远程服务器上改项目文件路径地址
    rootAllowed: '/',

    host: 'hostip',服务器地址
    port: 8345,
    pattern: '**',
    pollingInterval: 500,
    password: null,
    algorithm: 'aes128',

    onChange: function (type, path, oldPath) {
        // It can also return a promise.
        console.log('Write your custom code here')
    }
}

//注解：The pattern can be a string or an array, it takes advantage of minimatch.


```

github地址

https://github.com/ysmood/nobone-sync



