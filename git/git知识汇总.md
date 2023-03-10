
## 概念

#### 4个区

git之所以令人费解，主要是它相比于svn等等传统的版本管理工具，多引入了一个暂存区(Stage)的概念，就因为多了这一个概念，而使很多人疑惑。其实，在初学者来说，每个区具体怎么工作的，我们完全不需要关心，而只要知道有这么4个区就够了：

- 工作区(Working Area)
- 暂存区(Stage)
- 本地仓库(Local Repository)
- 远程仓库(Remote Repository)

#### 5种状态

以上4个区，进入每一个区成功之后会产生一个状态，再加上最初始的一个状态，一共是5种状态。以下我们把这5种状态分别命名为：

- 未修改(Origin)
- 已修改(Modified)
- 已暂存(Staged)
- 已提交(Committed)
- 已推送(Pushed)

#### 检查修改

###### 已修改，未暂存

```
git diff
```

###### 已暂存，未提交

```
git diff --cached
```

###### 已提交，未推送

```
git diff master origin/master
```

#### 撤销修改

了解清楚如何检查各种修改之后，我们开始尝试各种撤销操作。

###### 恢复已修改，未暂存

如果我们只是在编辑器里修改了文件，但还没有执行`git add .`，这时候我们的文件还在`工作区`，并没有进入`暂存区`，我们可以用撤销操作

```
git checkout .
```

或者

```
git reset --hard
```

###### 恢复已暂存，未提交

你已经执行了`git add .`，但还没有执行`git commit -m "comment"`。这时候你意识到了错误，想要撤销，你可以执行：

```
git reset
git checkout .
```

或者

```
git reset --hard
```

`git reset`只是把修改退回到了`git add .`之前的状态，也就是说文件本身还处于`已修改未暂存`状态，你如果想退回`未修改`状态，还需要执行`git checkout .`。

或许你已经注意到了，以上两个步骤都可以用同一个命令`git reset --hard`来完成。是的，就是这个强大的命令，可以一步到位地把你的修改完全恢复到`未修改`的状态。

###### 恢复已提交，未推送

你的手太快，你既执行了`git add .`，又执行了`git commit`，这时候你的代码已经进入了你的`本地仓库`，然而你后悔了，怎么办？不要着急，还有办法。

```
git reset --hard origin/master
```

还是这个`git reset --hard`命令，只不过这次多了一个参数`origin/master`，正如我们上面讲过的，`origin/master`代表`远程仓库`，既然你已经污染了你的`本地仓库`，那么就从`远程仓库`把代码取回来吧。

###### 已推送

很不幸，你的手实在是太快了，你既`git add`了，又`git commit`了，并且还`git push`了，这时你的代码已经进入`远程仓库`。如果你想恢复的话，还好，由于你的`本地仓库`和`远程仓库`是等价的，你只需要先恢复`本地仓库`，再强制`push`到`远程仓库`就好了：

```
git reset --hard HEAD^
git push -f
```

部分远程仓库设置了禁止强制push，我们可以使用以下方法。

######## revert

git revert用于反转提交。执行revert命令时要求工作树必须是干净的，git revert用一个新提交来消除一个历史提交所做的任何修改。revert 之后你的本地代码会回滚到指定的历史版本，这时你再 git push 就可以把线上的代码更新。



## 常用命令

| 命令                                            | 说明                                                                        |
| ----------------------------------------------- | --------------------------------------------------------------------------- |
| git clone                                       | 克隆                                                                        |
| git branch (分支名)                             | 创建分支 -D删除分支                                                         |
| git checkout (分支名)                           | 切换分支 -b创建并切换                                                       |
| git checkout (文件名)                           | 撤销此文件修改                                                              |
| git add                                         | 将该文件添加到缓存                                                          |
| git status                                      | 查看在你上次提交之后是否有修改                                              |
| git commit                                      | 将缓存区内容添加到仓库中                                                    |
| git diff                                        | 来查看执行 git status 的结果的详细信息                                      |
| git reset HEAD                                  | 命令用于取消已缓存的内容                                                    |
| git rm 文件                                     | 删除文件-f强制删除                                                          |
| git mv                                          | 移动文件                                                                    |
| git merge                                       | 合并分支                                                                    |
| git fetch                                       | 从远程获取最新版本到本地                                                    |
| git push <远程主机名> <本地分支名>:<远程分支名> | 将本地分支的更新，推送到远程主机                                            |
| git push origin --delete test                   | 删除远程分支test                                                            |
| git pull <远程主机名> <远程分支名>:<本地分支名> | 将远程存储库中的更改合并到本地分支中                                        |
| git rebase                                      | 命令在另一个分支基础之上重新应用，用于把一个分支的修改合并到当前分支。      |
| git log                                         | 命令用于显示提交日志信息。                                                  |
| git reflog                                      | 显示每一次命令                                                              |
| git revert                                      | 生成一个新的提交来撤销某次提交，此次提交之前的commit都会被保留              |
| git reset HEAD <file>                           | 如果发现错误的将不想暂存的文件被git add进入索引之后，想回退取消，则可以使用 |

设置用户名邮箱：

> git config --global [user.name](http://user.name/) "13sai"

> git config --global user.email ["957042781@qq.com](http://mailto:"957042781@qq.com/)"



###### 几个常见问题与解决方案

```
fatal: No configured push destination.

解决：
$ git remote add -f -t master -m master origin git://example.com/git.git/


fatal: The current branch test has no upstream branch.
解决：
git push --set-upstream origin test



模仿 git clone，但只跟踪选定的分支

$ mkdir project.git
$ cd project.git
$ git init
$ git remote add -f -t master -m master origin git://example.com/git.git/
$ git merge origin


fatal：Unable to create 'E:/project/scrm/.git/index.lock': File exists.

rm -f ./.git/index.lock
git log
-p 查看差异
-n(n为正整数) 查看最近n次的提交
--pretty        按指定格式显示日志信息,可选项有：oneline,short,medium,full,fuller,email,raw以及format:<string>,默认为medium，可以通过修改配置文件来指定默认的
方式。
e.g. git log (--pretty=)oneline
--stat  列出文件的修改行数
--sortstat      只显示--stat中最后行数修改添加移除的统计
--graph 以简单的图形方式列出提交记录
--abbrev-commit 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符。
--relative-date 使用较短的相对时间显示（比如，“2 weeks ago”）。
--name-only 仅在提交信息后显示已修改的文件清单。
--name-status 显示新增、修改、删除的文件清单。
```

######## 列出tag

> git tag ## 在控制台打印出当前仓库的所有tag

> git tag -l ‘v0.1.*’ ## 搜索符合模式的Tag 打tag

git tag分为两种类型：轻量tag和附注tag。轻量tag是指向提交对象的引用，附注Tag则是仓库中的一个独立对象。建议使用附注Tag。

######## 创建轻量Tag

> git tag v0.1.2-light

######## 创建附注Tag

> git tag -a v0.1.2 -m “0.1.2版本”

创建轻量Tag不需要传递参数，直接指定Tag名称即可。

创建附注Tag时，参数a即annotated的缩写，指定Tag类型，后附Tag名。参数m指定Tag说明，说明信息会保存在Tag对象中。

######## 切换到Tag

与切换分支命令相同，用

> git checkout [tagname]

######## 查看Tag信息

用git show命令可以查看Tag的版本信息：

> git show v0.1.2

######## 删除Tag

误打或需要修改Tag时，需要先将Tag删除，再打新Tag。

> git tag -d v0.1.2 ## 删除Tag

参数d即delete的缩写，意为删除其后指定的Tag。

给指定的commit打Tag 打Tag不必要在head之上，也可在之前的版本上打，这需要你知道某个提交对象的校验和（通过git log获取）。

######## 补打Tag

> git tag -a v0.1.1 9fbc3d0

Tag推送到服务器 通常的git push不会将Tag对象提交到git服务器，我们需要进行显式的操作：

> git push origin v0.1.2 ## 将v0.1.2 Tag提交到git服务器

> git push origin –-tags ## 将本地所有Tag一次性提交到git服务器

注意：如果想看之前某个Tag状态下的文件，可以这样操作

1. git tag 查看当前分支下的Tag
2. git checkout v0.21 此时会指向打v0.21 Tag时的代码状态，（但现在处于一个空的分支上）

######## 删除远程分支和tag

在Git v1.7.0 之后，可以使用这种语法删除远程分支：

> git push origin --delete <branchName>

删除tag这么用：

> git push origin --delete tag <tagname>

否则，可以使用这种语法，推送一个空分支到远程分支，其实就相当于删除远程分支：

> git push origin :<branchName>

这是删除tag的方法，推送一个空tag到远程tag：

> git tag -d <tagname>

> git push origin :refs/tags/<tagname>

######## 重命名远程分支

删除远程分支：

> git push --delete origin devel

重命名本地分支：

> git branch -m devel develop

推送本地分支：

> git push origin develop

######## 把本地tag推送到远程

> git push --tags ## 推送所有tag

> git push origin :tag ## 推送tag

