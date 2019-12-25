###git规范及常见问题解决

####注意规范

1.分支命名规范，提交信息应具有可辨识性



#####2..gitignore

```
在第一次推送代码之前，应当提前罗列出应当被忽略的文件
```



#####3.分支：

master分支应该是非常稳定的，即仅用来发布新版本，平常不能在此分支干活；我们每个人应该在各自的分支上进行开发，再进行合并分支，最后分支没问题了再合并到master上。

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

##### 4.多人协作工作模式

```
首先，可以试图用git push origin <branch-name>推送自己的修改；
如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
如果合并有冲突，则解决冲突，并在本地提交；
没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！
如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。
```

##### 5.当一个迭代功能完成之后，应当定期删除已经不必要的分支



####问题场景：

1.当前你正在dev分支开发一个功能，预计还有一天完成，这时你收到一个bug修复，务必两小时内完成，如何操作？

```powershell
git stash #将当前工作现场“储藏”起来，之后再使用git stash pop恢复
#确定要在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支：
git checkout master
git checkout -b issue101
#修复bug之后
git add bug.txt
git commit -m "fix bug 101"
#修复完成后，切换到master分支，并完成合并，最后删除issue-101分支：
git checkout master
git merge --no-ff -m "merged bug fix 101" issue101

#之后切回开发分支
#一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；

#另一种方式是用git stash pop，恢复的同时把stash内容也删了：

#再用git stash list查看，就看不到任何stash内容了

```

2.









#####Some small issue

1.

```
No user exists for uid 501
96  Bc_wh1te_Le1 关注
今天遇到的一个坑 提示No user exists for uid 501

No user exists for uid 501
fatal: Could not read from remote repository.
Please make sure you have the correct access rightsand the repository exists.
重启终端下就好 系统有更新的话 需要重启终端 更新下配置。
```

2.

```
git fatal: unable to access : Could not resolve host:
通过重新配置ssh就好了
```

3.

```
关于错误
fatal: unable to access 'https://git.coding.net/absir1/waternote.git/': Could not resolve proxy: git.coding.net

查看环境变量
$echo $http_proxy
$echo $https_proxy
$echo $HTTPS_PROXY
$echo $HTTP_PROXY
如果查看发现设置了则
 export https_proxy=
  export http_proxy=
```

4.

```
git reset--mixed（git提交错误，stash时提示merge。fatal: git-write-tree: error building treesCannot save the current index state）http://stackoverflow.com/questions/5483213/fatal-git-write-tree-error-building-trees
```

5.

```
git pull 失败 ,提示：fatal: refusing to merge unrelated histories

其实这个问题是因为 两个 根本不相干的 git 库， 一个是本地库， 一个是远端库， 然后本地要去推送到远端， 远端觉得这个本地库跟自己不相干， 所以告知无法合并

具体的方法， 一个种方法： 是 从远端库拉下来代码 ， 本地要加入的代码放到远端库下载到本地的库， 然后提交上去 ， 因为这样的话， 你基于的库就是远端的库， 这是一次update了


第二种方法：
使用这个强制的方法
git pull origin master --allow-unrelated-histories
后面加上 --allow-unrelated-histories ， 把两段不相干的 分支进行强行合并
后面再push就可以了 git push gitlab master:init
gitlab是别名 ， 使用

```

6.Git远程仓库地址变更本地如何修改

```
git remote set-url origin git@git.dev.tencent.com:huangzj08/wdy-resource-crm.git
```

解决关于Git无法提交 index.lock File exists的问题
今天提交代码时，在一次提交，莫名其妙没成功后，再次用git commit -a命令时，出现以下错误，
fatal: Unable to create 'XXXXXX/.git/index.lock': File exists.

if no other git process is currently running, this probably means a
git process crashed in this repository earlier. Make sure no other git
process is running and remove the file manually to continue.
解决办法:
删除.git文件下的index.lock 即可
