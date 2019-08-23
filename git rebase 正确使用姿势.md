在使用版本控制工具git管理项目中我们主要经常使用的git命令大致如下：

```
git status, git diff,git add, git stash, git push, git pull,git merge,git log,git blame ，git cherry-pick等
```

不知道小伙伴是否尝试了解和用过其他命令，比如  git rebase,如果知道并了解，欢迎阅读本篇并留言指正写得不对的地方，如果未了解，希望本篇可以给你带来git使用流程规范上的一些新体验。

#####使用git rebase的几种意义如下：

1.使用git log时，可以看到一个更整洁commit 历史树。

2.对于git工作流，commit数要多而有意义，这样review 代码速度加快，而且commit 都是有意义的，而且利于回退。

3.很多人的分支提交历史经常是fix，fix, fix agian,好的commit应该反映出项目的一步步开发。

下面主要介绍git rebase在工作中有用的几种场景。

#####1.与远程代码同步时使用git pull --rebase

当我们项目多人在同一个分支开发时，不可避免在你要提交代码之前有人已往远程仓库提交代码。当我们push时，提示你先pull，如果我们直接pull，有冲突的情况我们需要解决冲突，没有冲突的情况自动合并，不管哪种情况我们都会产生一条新的提交记录

![图片](https://coding-net-production-pp-ci.codehub.cn/518e9a5a-1813-4964-b2d6-80979ca6363f.png)

Push 远程之后可以看到这样的提交历史

![图片](https://coding-net-production-pp-ci.codehub.cn/e4e1b424-5034-4806-9349-b7e78cc8a1df.png)

如何避免这样一条多余的提交记录呢？我们使用git pull rebase

再次模拟场景。

![图片](https://coding-net-production-pp-ci.codehub.cn/3637daf9-35ff-4a96-a266-766c41eb4268.png)

使用git log查看，没有多余的merge记录





Ok,我们看另一种情况，当两个人同时修改了一个文件，使用git pull --rebase怎么处理

![图片](https://coding-net-production-pp-ci.codehub.cn/5360aa58-94b4-4a6c-9f61-7b2dfb907309.png)

提示有冲突，于是我们去解决冲突，之后我们使用git add  ，再使用 git rebase --continue

![图片](https://coding-net-production-pp-ci.codehub.cn/18d1e305-f75e-4612-8150-205b5aed078e.png)

然后我们使用git log查看发现此次提交记录放到了当前远程分支最新提交记录之后，提交历史很整洁没有多余的commit记录。我们还可以看到使用rebase时游离分支917f927其实就是上一条提交记录id。这条神奇操作就是Git把我们本地的提交“挪动”了位置，放到了917f927 (origin/master) `之后。

![图片](https://coding-net-production-pp-ci.codehub.cn/8ea40476-d17e-4b8c-a145-5dcf975eb392.png)

##### 补充说明：

1.git rebase --continue 当冲突问题解决之后可以使用git add .再使用该命令继续完成

2.git rebase --skip 如果你想丢弃该条引起冲突的commits，可以使用该命令，慎用；

3.git rebase --abort 当前的commit会回到rebase操作之前的状态。

```
git pull的默认行为是git fetch + git merge
git pull --rebase则是git fetch + git rebase.
git fetch:从远程获取最新版本到本地，不会自动合并分支
```

#####2.整合分支

提到整合分支，可以想到最容易的方法是merge命令。它会把两个分支的最新快照（`C3` 和 `C4`）以及二者最近的共同祖先（`C2`）进行三方合并，合并的结果是生成一个新的快照（并提交）。

![éè¿åå¹¶æä½æ¥æ´åååäºçåå²ã](https://git-scm.com/book/en/v2/images/basic-rebase-2.png)



我们通过一个例子来说明：(暂时先忽略需要解决冲突的情况)

在当前你的master分支切出一个新分支test(git checkout -b test),增加一个文件，而此时master上有个小东西需要直接改一下，你接下来切回master分支，你修改了一些提交commit "master修改"并push远程。之后回到test分支继续开发完成，之后commit提交"测试合并",push远程。

接下来，test分支没问题了，你需要合并test分支到master。通常我们最多的是在master分支上使用git merge test 命令，这时master会产生一条新的commit记录，默认叫 Merge branch 'test'.你可以修改它，之后push远程,查看远程提交历史如下：

![图片](https://coding-net-production-pp-ci.codehub.cn/79b565de-9ac9-416b-9835-6be66b30ecd3.png)

对于一些在git log --graph的时候只想看到流水线完整提交记录的人来说，使用git rebase来整合分支会更合适。

**使用 `rebase` 命令将提交到某一分支上的所有修改都移至另一分支上.**

> 补充说明：如果只是复制某一两个提交到其他分支，建议使用更简单的命令:`git cherry-pick`

仍然以刚才的例子说明，

```
git checkout test 添加一个文件,commit之后，我们使用git rebase master,之后回master分支，使用git merge test进行一次快进合并.
```

使用git log 查看test上的commit记录，和上面直接使用merge的方式不同，会少一条merge记录。

这两种整合方法的最终结果没有任何区别，但是变基使得**提交历史更加整洁**。在查看一个经过变基的分支的历史记录时会发现，尽管实际的开发工作是并行的，但它们看上去就像是串行的一样，提交历史是一条直线没有分叉。



一般我们这样做的目的是为了确保在向远程分支推送时能保持提交历史的整洁——例如向某个其他人维护的项目贡献代码时。 在这种情况下，你首先在自己的分支里进行开发，当开发完成时你需要先将你的代码变基到 `origin/master` 上，然后再向主项目提交修改. 这样的话，该项目的维护者就不再需要进行整合工作，只需要快进合并便可。

变基是将一系列提交按照原有次序依次应用到另一分支上，而合并是把最终结果合在一起。

```
这里补充一个命令git cherry-pick。
可以选择某一个分支中的一个或几个commit(s)来进行操作（操作的对象是commit）。
假设我们有个稳定版本的分支，叫v2.0，另外还有个开发版本的分支v3.0，我们不能直接把两个分支合并，这样会导致稳定版本混乱，但是又想增加一个v3.0中的功能到v2.0中，这里就可以使用cherry-pick了。
git log查看版本历史

```



#####3.合并多个commit

当我们在修改一个需求时，对修复同一个问题提交了多个相似commit,如下

![图片](https://coding-net-production-pp-ci.codehub.cn/3bcac173-6cb5-4836-82c3-46c11fdf984c.png)

如果直接push到远程，会显得很啰嗦而且很乱，这时我们可以合并这几个commit，使此次提交更简练。

我们使用命令

```
git rebase -i  [startpoint]  [endpoint]
其中-i的意思是--interactive，即弹出交互式的界面让用户编辑完成合并操作，[startpoint]  [endpoint]则指定了一个编辑区间，如果不指定[endpoint]，则该区间的终点默认是当前分支HEAD所指向的commit(注：该区间指定的是一个前开后闭的区间)。
git rebase -i f4c477^ （使用^表示从该commitid开始）
```

```shell
或者git rebase -i HEAD~3
```

进入一个编辑模式界面：

![图片](https://coding-net-production-pp-ci.codehub.cn/9aaf7534-0840-4484-8fb1-40ce844f7245.png)

> 补充说明:

```
pick：保留该commit（缩写:p）
reword：保留该commit，但我需要修改该commit的注释（缩写:r）
edit：保留该commit, 但我要停下来修改该提交(不仅仅修改注释)（缩写:e）
squash：将该commit和前一个commit合并（缩写:s）
fixup：将该commit和前一个commit合并，但我不要保留该提交的注释信息（缩写:f）
exec：执行shell命令（缩写:x）
drop：我要丢弃该commit（缩写:d）
```

根据我们需求，我们修改commit内容如下

![图片](https://coding-net-production-pp-ci.codehub.cn/473b63cc-f2d0-41b7-8ecd-efa91b1edde5.png)

```
esc :wq之后进入另一个编辑模式页面
```

![图片](https://coding-net-production-pp-ci.codehub.cn/e34d571e-b2a2-438b-aee9-485b8207f901.png)

浏览态下按下两个dd可以删除一行，编辑成如下

![图片](https://coding-net-production-pp-ci.codehub.cn/f007aaa1-3340-44d8-bfd6-3e7fbaf0a1a4.png)

然后保存查看 git log

![图片](https://coding-net-production-pp-ci.codehub.cn/28d3cc33-8fff-4b00-a326-03b7ccb3f424.png)

ok



#####最后补充说明事项:

不要通过rebase对任何已经提交到公共仓库中的commit进行修改,自己一个人玩的分支除外.

只要你把变基命令当作是在推送前清理提交使之整洁的工具，并且只在从未推送至共用仓库的提交上执行变基命令，就不会有事。 假如在那些已经被推送至共用仓库的提交上执行变基命令，并因此丢弃了一些别人的开发所基于的提交，那你就麻烦了.

######报错
我初始化项目，还未提交远程第一次
在使用git rebase  -i HEAD～xxx出现以下错误

```
fatal: Needed a single revision
invalid upstream HEAD^3
b8ea8e97d
9396b61be
31a76abff
```

错误原因

1. 当前执行操作的点不在任何分支上，或者可能rebase后面的参数是一个错误的分支；
2. 当前执行操作的点前面的提交不够3个。

解决办法

1.确认'-i' 之后的参数是否正确；

2.确认需要rebase的提交相对于'HEAD'的序号，一种极端情况是想从当前分支的第一个提交开始rebase，可以使用以下命令：git rebase -i --root。

When I used git `rebase –i HEAD~2`, 或 git rebase -i --root
```
Successfully rebased and updated refs/heads/master.
```
参考文章：

```
https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%8F%98%E5%9F%BA
https://git-scm.com/docs/git-rebase
https://www.jianshu.com/p/4a8f4af4e803
```
