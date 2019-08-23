#### git 基本常用命令详解

##### 一.命令简介

```powershell
git init
git clone url
git status
git status -s
git add file #把文件修改添加到暂存区
git diff
git diff HEAD -- readme.txt#查看工作区和版本库里面最新版本的区别：
git diff --cached
git commit -m "" #把暂存区的所有内容提交到当前分支
git commit -am ""
git rm file #移除文件，下一次提交时，该文件就不再纳入版本管理了
git rm --cached file #把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中
git mv file_from file_to #修改文件名
git log #查看提交历史
git log -2 #历史表示近两次提交
git log -p -2 #常用的选项是 -p，用来显示每次提交的内容差异
git log --stat #粗略查看每次提交的简略的统计信息
git log --author=zxxydy #显示指定作者的提交
git commit --amend #撤销提交
git reset HEAD~     取消暂存区最靠前的一次提交
git reset HEAD file #取消暂存的文件（暂存区）
git checkout -- file #直接丢弃工作区的修改(工作区) (撤销还原操作)
git reset --hard HEAD^ #回退上一个版本
git reset --hard commit_id #撤销回到新的版本，commit id = 1094a
git reflog #查看命令历史

git remote add origin [git-url]#将本地库和远程库关联起来
git push -u origin master# 关联后，使用命令git push -u origin master第一次推送master分支的所有内容；此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；

git checkout -b dev #创建dev分支并切换到dev分支
git branch #查看当前分支,该命令会列出所有分支，当前分支前会标一个*
git branch -av #查看分支详细情况 （推荐这种方式）
git merge #合并指定分支到当前分支
git branch -d dev #删除分支
git merge dev #合并分支
git log --graph #查看分支合并图
git remote -v #查看远程库信息
git push origin branch-name #从本地推送分支
#如果推送失败，先用git pull抓取远程的新提交；

git tag v0.1 #打标签，标签总和某个commit挂钩
git tag -d v0.1 #删除标签（本地未推送）
git push origin :refs/tags/v0.9 #删除标签（已推送远程，需多一步）
git push origin v1.0 #推送标签到远程
git fetch [remote-name]#这个命令访问远程仓库，从中拉取所有你还没有的数据。
git remote add <shortname> <url>#添加一个新的远程 Git 仓库，同时指定一个你可以轻松引用的简写：
git fetch origin #抓取克隆(或上一次抓取)后新推送的所有工作
git pull
git push

git 删除分支并推送远程分支
【git 删除本地分支】
git branch -D br
【git 删除远程分支】
git push origin :br  (origin 后面有空格)
```



####二.命令详解

##### 1.获取git项目仓库

```powershell
#两种方法
1.git init
2.git clone [url]
```

> 方法1：在现有目录中初始化仓库

如果打算使用 Git 来对现有的项目进行管理，那么执行如下命令即可

```powershell
mkdir gitdemo #新建一个现有文件项目
cd gitdemo
git init #该命令将项目目录下创建一个名为.git的子目录，使用ls -a即可查看
```

如果你是在一个已经存在文件的文件夹（而不是空文件夹）中初始化 Git 仓库来进行版本控制的话，你应该开始跟踪这些文件并提交。 你可通过 `git add` 命令来实现对指定文件的跟踪，然后执行 `git commit`提交.

> 方法2:使用 git clone命令

```powershell
#比如从github上找一个项目
$ git clone https://github.com/leon-ai/leon.git
#这会在当前目录下创建一个名为leon的目录，并在这个目录下初始化一个 .git 文件夹
#如果你想在克隆远程仓库的时候，自定义本地仓库的名字，你可以使用如下命令：
$ git clone https://github.com/leon-ai/leon.git  myleon
```

![图片](https://dn-coding-net-production-pp.codehub.cn/0decc185-bd12-47f1-adf7-89ce52a53a09.png)

##### 2.跟踪文件并提交

工作目录下的每一个文件都不外乎这两种状态：已跟踪或未跟踪.

已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能处于未修改，已修改或已放入暂存区。

工作目录中除已跟踪文件以外的所有其它文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有放入暂存区。

 初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态。

![Git ä¸æä"¶çå½å¨æå¾ã](https://git-scm.com/book/en/v2/images/lifecycle.png)

###### 检查当前文件状态

```
git status
```

 1.如果在克隆仓库后立即使用此命令,可看到如下输出

```powershell
$ git status
On branch master
nothing to commit, working directory clean
#//这说明你现在的工作目录相当干净。换句话说，所有已跟踪文件在上次提交后都未被更改过。 此外，上面的信息还表明，当前目录下没有出现任何处于未跟踪状态的新文件，否则 Git 会在这里列出来。 最后，该命令还显示了当前所在分支
```

2.创建一个新的 README 文件。 如果之前并不存在这个文件，使用 `git status`命令，你将看到一个新的未跟踪文件：

```powershell
Untracked files:
  (use "git add <file>..." to include in what will be committed)

       	1.txt

nothing added to commit but untracked files present (use "git add" to track)
```

###### 跟踪新文件

```powershell
git add 1.txt
#再运行git status会看到
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

       	new file:   1.txt
 #只要在 Changes to be committed 这行下面的，就说明是已暂存状态
```

`git add` 命令使用文件或目录的路径作为参数；如果参数是目录的路径，该命令将递归地跟踪该目录下的所有文件。

```powershell
#暂存已修改文件，如下修改了1.txt,已跟踪文件的内容发生了变化，但还没有放到暂存区。 要暂存这次更新，需要运行 git add 命令
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

       	new file:   1.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

       	modified:   1.txt
```

命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。

git add 可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等

使用 `git status -s` 命令或 `git status --short` 命令，你将得到一种更为紧凑的格式输出。

![图片](https://dn-coding-net-production-pp.codehub.cn/613169cc-9f23-45c6-88a3-c363273f081c.png)

**新添加的未跟踪文件前面有 `??` 标记，新添加到暂存区中的文件前面有 `A` 标记，修改过的文件前面有 `M` 标记。**

`M` 有两个可以出现的位置，出现在右边的 `M` 表示该文件被修改了但是还没放入暂存区，出现在靠左边的 `M` 表示该文件被修改了并放入了暂存区

![图片](https://dn-coding-net-production-pp.codehub.cn/2a8a3418-b9ef-49f8-8a62-c4d4c2b4b48a.png)

##### 查看已暂存和未暂存的修改

```powershell
git diff
#查看具体修改了哪些地方，可以使用git diff命令，可以知道当前做的哪些更新还没有暂存？ 有哪些更新已经暂存起来准备好了下次提交？
```

若要查看已暂存的将要添加到下次提交里的内容，可以用 `git diff --cached` 命令。

 使用 `git difftool --tool-help` 命令来看你的系统支持哪些 Git Diff 插件。

##### 提交更新

在提交之前请一定要确认还有什么修改过的或新建的文件还没有 `git add` 过，否则提交的时候不会记录这些还没暂存起来的变化。

```
git commit
#这种方式会启动文本编辑器以便输入本次提交的说明
#Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# Changes to be committed:
#	new file:   README
#	modified:   CONTRIBUTING.md
#
~
~
~
".git/COMMIT_EDITMSG" 9L, 283C
```

在 `commit` 命令后添加 `-m` 选项，将提交信息与命令放在同一行

```powershell
$ git commit -m "Story 182: Fix benchmarks for speed"
[master 463dc4f] Story 182: Fix benchmarks for speed
 2 files changed, 2 insertions(+)
 create mode 100644 README
 #提交后它会告诉你，当前是在哪个分支（master）提交的，本次提交的完整 SHA-1 校验和是什么（463dc4f），以及在本次提交中，有多少文件修订过，多少行添加和删改过。
#每一次运行提交操作，都是对你项目作一次快照，以后可以回到这个状态，或者进行比较。
```

#####跳过使用暂存区域

给 `git commit` 加上 `-a` 选项，Git 就会自动把所有**已经跟踪过的文件**暂存起来一并提交，(未跟踪过的文件不可)

```
git commit -am "xxx"
```

#####移除文件

要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。 可以用 `git rm` 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。

下一次提交时，该文件就不再纳入版本管理了。 如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 `-f`（译注：即 force 的首字母）

另外一种情况是，我们想把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中。 换句话说，你想让文件保留在磁盘，但是并不想让 Git 继续跟踪。 当你忘记添加 `.gitignore`文件，不小心把一个很大的日志文件或一堆 `.a` 这样的编译生成文件添加到暂存区时，这一做法尤其有用。 为达到这一目的，使用 `--cached` 选项：

```console
$ git rm --cached README
```

`git rm` 命令后面可以列出文件或者目录的名字，也可以使用 `glob` 模式。 比方说：

```console
$ git rm log/\*.log
```

##### 移动文件

```powershell
git mv README.md README
=>
# git mv README.md README
#$ git status
#On branch master
#Changes to be committed:
#  (use "git reset HEAD <file>..." to unstage)

#    renamed:    README.md -> README
=>等价于
$ mv README.md README
$ git rm README.md
$ git add README
```

####3.查看历史

```
git log
```

![图片](https://dn-coding-net-production-pp.codehub.cn/64d5c445-3e4e-4559-ab53-1d540ca23afd.png)

默认不用任何参数的话，`git log` 会按提交时间列出所有的更新，最近的更新排在最上面

默认不用任何参数的话，`git log` 会按提交时间列出所有的更新，最近的更新排在最上面。 正如你所看到的，这个命令会列出每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明。

`git log` 有许多选项可以帮助你搜寻你所要找的提交， 接下来我们介绍些最常用的。

一个常用的选项是 `-p`，用来显示每次提交的内容差异。 你也可以加上 `-2` 来仅显示最近两次提交：

该选项除了显示基本信息之外，还附带了每次 commit 的变化。 当进行代码审查，或者快速浏览某个搭档提交的 commit 所带来的变化的时候，这个参数就非常有用了。 你也可以为 `git log` 附带一系列的总结性选项。 比如说，如果你想看到每次提交的简略的统计信息，你可以使用 `--stat` 选项：

```
git log --stat
```

![图片](https://dn-coding-net-production-pp.codehub.cn/7a9e8feb-46eb-42f5-82a9-c5fc73e813d4.png)

####4.撤销操作

学习几个撤消你所做修改的基本工具。 注意，有些撤消操作是不可逆的。 这是在使用 Git 的过程中，会因为操作失误而导致之前的工作丢失的少有的几个地方之一。

有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。 此时，可以运行带有 `--amend` 选项的提交命令尝试重新提交：

```
git commit --amend
```

你提交后发现忘记了暂存某些需要的修改，可以像下面这样操作：

```console
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```

最终你只会有一个提交 - 第二次提交将代替第一次提交的结果。

#####取消暂存的文件

如果你已经修改了两个文件并且想要将它们作为两次独立的修改提交，但是却意外地输入了 `git add *` 暂存了它们两个。 如何只取消暂存两个中的一个呢？

```
git reset HEAD 2.txt
```

#####撤消对文件的修改

如果你并不想保留对 `CONTRIBUTING.md` 文件的修改,如何方便地撤消修改 - 将它还原成上次提交时的样子

```powershell
git checkout -- CONTRIBUTING.md
#git checkout -- [file] 是一个危险的命令，这很重要。 你对那个文件做的任何修改都会消失 - 你只是拷贝了另一个文件来覆盖它。 除非你确实清楚不想要那个文件了，否则不要使用这个命令。
```

####5.管理远程仓库

通常有些仓库对你只读，有些则可以读写。

 管理远程仓库包括了解如何添加远程仓库、移除无效的远程仓库、管理不同的远程分支并定义它们是否被跟踪等等

##### 查看远程仓库

```powershell
git remote
=>origin

git remote -v
=>
#origin	https://github.com/schacon/ticgit (fetch)
#origin	https://github.com/schacon/ticgit (push)
```

##### 添加远程仓库

```powershell
 #可以通过git remote add <shortname> <url>添加一个新的远程git仓库，同时指定一个你可以轻松引用的简写：
 eg:
 git remote add pb https://github.com/paulboone/ticgit
```

现在你可以在命令行中使用字符串 `pb` 来代替整个 URL。 例如，如果你想拉取 Paul 的仓库中有但你没有的信息，可以运行 `git fetch pb`：

```
git fetch pb
remote: Enumerating objects: 634, done.
remote: Total 634 (delta 0), reused 0 (delta 0), pack-reused 634
Receiving objects: 100% (634/634), 88.92 KiB | 121.00 KiB/s, done.
Resolving deltas: 100% (261/261), done.
From https://github.com/paulboone/ticgit
 * [new branch]      master     -> pb/master
 * [new branch]      ticgit     -> pb/ticgit
```

##### 从远程仓库中抓取与拉取

```powershell
$ git fetch [remote-name]
#这个命令会访问远程仓库，从中拉取所有你还没有的数据。 执行完成后，你将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。
```

如果你使用 `clone` 命令克隆了一个仓库，命令会自动将其添加为远程仓库并默认以 “origin” 为简写。

`git fetch origin` 会抓取克隆（或上一次抓取）后新推送的所有工作.

`git fetch` 命令会将数据拉取到你的本地仓库 - 它并不会自动合并或修改你当前的工作.

如果你有一个分支设置为跟踪一个远程分支，可以使用 `git pull` 命令来自动的抓取然后合并远程分支到当前分支.

#####推送到远程仓库

```powershell
git push [remote-name] [branch-name]
git push origin master
```

##### 查看远程仓库

```powershell
git remote show origin
#这个命令列出了当你在特定的分支上执行 git push 会自动地推送到哪一个远程分支。 它也同样地列出了哪些远程分支不在你的本地，哪些远程分支已经从服务器上移除了，还有当你执行 git pull 时哪些分支会自动合并。
```

##### 远程仓库的移除与重命名

```powershell
git remote rename pb paul
git remote #查看远程仓库名
git remote rm paul #移除远程仓库
```

####6.打标签

Git 可以给历史中的某一个提交打上标签，以示重要.

 比较有代表性的是会使用这个功能来标记发布结点（v1.0 等等）

###### 列出标签

```powershell
git tag
#可以使用特定的模式查找标签
git tag -1 'v1.8.5*'
```

###### 创建标签 :轻量标签&&附注标签

1.附注标签:附注标签其中包含打标签者的名字、电子邮件地址、日期时间；还有一个标签信息

```powershell
 git tag -a v1.4 -m 'my version 1.4'
 git tag
 =>
 v0.1
 v1.3
 v1.4
#-m 选项指定了一条将会存储在标签中的信息,如果没有为附注标签指定一条信息，Git 会运行编辑器要求你输入信息。
```

通过使用 `git show` 命令可以看到标签信息与对应的提交信息：

```
git show v1.4
tag v1.4
Tagger: Ben Straub <ben@straub.cc>
Date:   Sat May 3 20:19:12 2014 -0700

my version 1.4

commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number
```

2.轻量标签

轻量标签本质上是将提交校验和存储到一个文件中 - 没有保存任何其他信息。 创建轻量标签，不需要使用 `-a`、`-s` 或 `-m` 选项，只需要提供标签名字：

```
git tag v1.4
在标签上运行 git show，你不会看到额外的标签信息。 命令只会显示出提交信息
```

###### 后期打标签

可以对过去的提交打标签,假设提交历史是这样的

```console
git log --pretty=oneline
15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'
a6b4c97498bd301d84096da251c98a07c7723e65 beginning write support
0d52aaab4479697da7686c15f77a3d64d9165190 one more thing
6d52a271eda8725415634dd79daabbc4d9b6008e Merge branch 'experiment'
0b7434d86859cc7b8c3d5e1dddfed66ff742fcbc added a commit function
4682c3261057305bdd616e23b64b0857d832627b added a todo file
166ae0c4d3f420721acbb115cc33848dfcc2121a started write support
9fceb02d0ae598e95dc970b74767f19372d61af8 updated rakefile
964f16d36dfccde844893cac5b347e7b3d44abbc commit the todo
8a5cbc430f1a9c3d00faaeffd07798508422908a updated readme
```

现在，假设在 v1.2 时你忘记给项目打标签，也就是在 “updated rakefile” 提交。 你可以在之后补上标签。 要在那个提交上打标签，你需要在命令的末尾指定提交的校验和（或部分校验和）:

```console
git tag -a v1.2 9fceb02
```

###### 远程推送标签 `git push origin [tagname]`。

```powershell
git push origin v1.5
git push origin --tags #把所有不在远程仓库服务器上的标签全部传送到那里
```

###### 删除标签

```powershell
git tag -d <tagname>
#然后使用git push <remote> :refs/tags/<tagname> 来更新你的远程仓库：
```

#### 7.分支管理

#####分支创建

```console
git branch testing
```

#####分支切换

```console
git checkout testing
```

##### 分支的新建与合并

1. 切换到你的线上分支（production branch）。
2. 为这个紧急任务新建一个分支，并在其中修复它。
3. 在测试通过之后，切换回线上分支，然后合并这个修补分支，最后将改动推送到线上分支。
4. 切换回你最初工作的分支上，继续工作。

```
git checkout -b iss53 新建并切换新分支
```

在做了一些修改并提交之后，收到紧急问题修复通知,这时仅切回master分支即可，但是，在你这么做之前，要留意你的工作目录和暂存区里那些还没有被提交的修改，它可能会和你即将检出的分支产生冲突从而阻止 Git 切换到该分支。 最好的方法是，在你切换分支之前，保持好一个干净的状态。 有一些方法可以绕过这个问题（即，保存进度（stashing） 和 修补提交（commit amending）），

```
git checkout master
这个时候，你的工作目录和你在开始 #53 问题之前一模一样，现在你可以专心修复紧急问题了.当你切换分支的时候，Git 会重置你的工作目录，使其看起来像回到了你在那个分支上最后一次提交的样子
```

##### 分支管理

```powershell
git branch -v #看每一个分支的最后一次提交
```

`--merged` 与 `--no-merged` 这两个有用的选项可以过滤这个列表中已经合并或尚未合并到当前分支的分支。 如果要查看哪些分支已经合并到当前分支，可以运行 `git branch --merged`：

#### 8.变基

在git中整合来自不同分支的修改主要有两种方法：merge以及rebase。

##### 变基的基本操作

![ååçæäº¤åå²ã](https://git-scm.com/book/en/v2/images/basic-rebase-1.png)

整合分支最容易的方法是 `merge` 命令。 它会把两个分支的最新快照（`C3` 和 `C4`）以及二者最近的共同祖先（`C2`）进行三方合并，合并的结果是生成一个新的快照（并提交）。

其实，还有一种方法：你可以提取在 `C4` 中引入的补丁和修改，然后在 `C3` 的基础上应用一次。 在 Git 中，这种操作就叫做 *变基*。

可以使用 `rebase` 命令将提交到某一分支上的所有修改都移至另一分支上，就好像“重新播放”一样。

```
git checkout experiment
git rebase master

git checkout master
git merge experiment
变基使得提交历史更加整洁
```

