#### 解决方案

**1.执行下面命令**

```
git config --global credential.helper store
```

> 这个时候~/.gitconfig文件中会多一行
>
> [credential]
>  helper = store

**2.执行git pull再次输入用户名和密码**

此时你会看到`/root/.git-credentials`中会多一行内容。里面的内容类似`https://{username}:{password}@github.com`这种形式

作者：缘来是你ylh

链接：https://www.jianshu.com/p/faba73a28a7c

来源：简书

简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。