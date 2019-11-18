---
title: Git的分支操作
tags: Git操作总结
---

## [git clone，push，pull，fetch命令详解（转载）](https://www.cnblogs.com/xiaopangjr/p/7469687.html)

 

Git是目前最流行的版本管理系统，学会Git几乎成了开发者的必备技能。

Git有很多优势，其中之一就是远程操作非常简便。本文详细介绍5个Git命令，它们的概念和用法，理解了这些内容，你就会完全掌握Git远程操作。

- git clone
- git remote
- git fetch
- git pull
- git push

本文针对初级用户，从最简单的讲起，但是需要读者对Git的基本用法有所了解。同时，本文覆盖了上面5个命令的几乎所有的常用用法，所以对于熟练用户也有参考价值。

## 一、git clone

远程操作的第一步，通常是从远程主机克隆一个版本库，这时就要用到`git clone`命令。

> ```bash
> $ git clone <版本库的网址>
> ```

比如，克隆[jQuery](http://lib.csdn.net/base/jquery)的版本库。

> ```bash
> $ git clone https://github.com/jquery/jquery.git
> ```

该命令会在本地主机生成一个目录，与远程主机的版本库同名。如果要指定不同的目录名，可以将目录名作为`git clone`命令的第二个参数。

> ```bash
> $ git clone <版本库的网址> <本地目录名>
> ```

`git clone`支持多种协议，除了HTTP(s)以外，还支持SSH、Git、本地文件协议等，下面是一些例子。

> ```bash
> $ git clone http[s]://example.com/path/to/repo.git/
> $ git clone ssh://example.com/path/to/repo.git/
> $ git clone git://example.com/path/to/repo.git/
> $ git clone /opt/git/project.git 
> $ git clone file:///opt/git/project.git
> $ git clone ftp[s]://example.com/path/to/repo.git/
> $ git clone rsync://example.com/path/to/repo.git/
> ```

SSH协议还有另一种写法。

> ```bash
> $ git clone [user@]example.com:path/to/repo.git/
> ```

通常来说，Git协议下载速度最快，SSH协议用于需要用户认证的场合。各种协议优劣的详细讨论请参考[官方文档](http://git-scm.com/book/en/Git-on-the-Server-The-Protocols)。

## 二、git remote

为了便于管理，Git要求每个远程主机都必须指定一个主机名。`git remote`命令就用于管理主机名。

不带选项的时候，`git remote`命令列出所有远程主机。

> ```bash
> $ git remote
> origin
> ```

使用`-v`选项，可以参看远程主机的网址。

> ```bash
> $ git remote -v
> origin  git@github.com:jquery/jquery.git (fetch)
> origin  git@github.com:jquery/jquery.git (push)
> ```

上面命令表示，当前只有一台远程主机，叫做origin，以及它的网址。

克隆版本库的时候，所使用的远程主机自动被Git命名为`origin`。如果想用其他的主机名，需要用`git clone`命令的`-o`选项指定。

> ```bash
> $ git clone -o jQuery https://github.com/jquery/jquery.git
> $ git remote
> jQuery
> ```

上面命令表示，克隆的时候，指定远程主机叫做[jquery](http://lib.csdn.net/base/jquery)。

`git remote show`命令加上主机名，可以查看该主机的详细信息。

> ```bash
> $ git remote show <主机名>
> ```

`git remote add`命令用于添加远程主机。

> ```bash
> $ git remote add <主机名> <网址>
> ```

`git remote rm`命令用于删除远程主机。

> ```bash
> $ git remote rm <主机名>
> ```

`git remote rename`命令用于远程主机的改名。

> ```bash
> $ git remote rename <原主机名> <新主机名>
> ```

## 三、git fetch

一旦远程主机的版本库有了更新（Git术语叫做commit），需要将这些更新取回本地，这时就要用到`git fetch`命令。

> ```bash
> $ git fetch <远程主机名>
> ```

上面命令将某个远程主机的更新，全部取回本地。

`git fetch`命令通常用来查看其他人的进程，因为它取回的代码对你本地的开发代码没有影响。

默认情况下，`git fetch`取回所有分支（branch）的更新。如果只想取回特定分支的更新，可以指定分支名。

> ```bash
> $ git fetch <远程主机名> <分支名>
> ```

比如，取回`origin`主机的`master`分支。

> ```bash
> $ git fetch origin master
> ```

所取回的更新，在本地主机上要用"远程主机名/分支名"的形式读取。比如`origin`主机的`master`，就要用`origin/master`读取。

`git branch`命令的`-r`选项，可以用来查看远程分支，`-a`选项查看所有分支。

> ```bash
> $ git branch -r
> origin/master
> 
> $ git branch -a
> * master
> remotes/origin/master
> ```

上面命令表示，本地主机的当前分支是`master`，远程分支是`origin/master`。

取回远程主机的更新以后，可以在它的基础上，使用`git checkout`命令创建一个新的分支。

> ```bash
> $ git checkout -b newBrach origin/master
> ```

上面命令表示，在`origin/master`的基础上，创建一个新分支。

此外，也可以使用`git merge`命令或者`git rebase`命令，在本地分支上合并远程分支。

> ```bash
> $ git merge origin/master
> # 或者
> $ git rebase origin/master
> ```

上面命令表示在当前分支上，合并`origin/master`。

## 四、git pull

`git pull`命令的作用是，取回远程主机某个分支的更新，再与本地的指定分支合并。它的完整格式稍稍有点复杂。

> ```bash
> $ git pull <远程主机名> <远程分支名>:<本地分支名>
> ```

比如，取回`origin`主机的`next`分支，与本地的`master`分支合并，需要写成下面这样。

> ```bash
> $ git pull origin next:master
> ```

如果远程分支是与当前分支合并，则冒号后面的部分可以省略。

> ```bash
> $ git pull origin next
> ```

上面命令表示，取回`origin/next`分支，再与当前分支合并。实质上，这等同于先做`git fetch`，再做`git merge`。

> ```bash
> $ git fetch origin
> $ git merge origin/next
> ```

在某些场合，Git会自动在本地分支与远程分支之间，建立一种追踪关系（tracking）。比如，在`git clone`的时候，所有本地分支默认与远程主机的同名分支，建立追踪关系，也就是说，本地的`master`分支自动"追踪"`origin/master`分支。

Git也允许手动建立追踪关系。

> ```bash
> $ git branch --set-upstream master origin/next
> ```

上面命令指定`master`分支追踪`origin/next`分支。

如果当前分支与远程分支存在追踪关系，`git pull`就可以省略远程分支名。

> ```bash
> $ git pull origin
> ```

上面命令表示，本地的当前分支自动与对应的`origin`主机"追踪分支"（remote-tracking branch）进行合并。

如果当前分支只有一个追踪分支，连远程主机名都可以省略。

> ```bash
> $ git pull
> ```

上面命令表示，当前分支自动与唯一一个追踪分支进行合并。

如果合并需要采用rebase模式，可以使用`--rebase`选项。

> ```bash
> $ git pull --rebase <远程主机名> <远程分支名>:<本地分支名>
> ```

如果远程主机删除了某个分支，默认情况下，`git pull` 不会在拉取远程分支的时候，删除对应的本地分支。这是为了防止，由于其他人操作了远程主机，导致`git pull`不知不觉删除了本地分支。

但是，你可以改变这个行为，加上参数 `-p` 就会在本地删除远程已经删除的分支。

> ```bash
> $ git pull -p
> # 等同于下面的命令
> $ git fetch --prune origin 
> $ git fetch -p
> ```

## 五、git push

`git push`命令用于将本地分支的更新，推送到远程主机。它的格式与`git pull`命令相仿。

```bash
git push <远程主机名> <本地分支名>:<远程分支名>
git push <远程主机名> <本地分支名> # 本地分支名和远程分支名相同时，远程分支名可省略
git push <远程主机名>
```



注意，分支推送顺序的写法是<来源地>:<目的地>，所以`git pull`是<远程分支>:<本地分支>，而`git push`是<本地分支>:<远程分支>。



> ```bash
> $ git push origin hexo
> ```

上面命令表示，将本地的`hexo`分支推送到`origin`主机的hexo分支。如果后者不存在，则会被新建。

如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支。

> ```bash
> $ git push origin :master
> # 等同于
> $ git push origin --delete master
> ```

上面命令表示删除`origin`主机的`master`分支。

如果当前分支与远程分支之间存在追踪关系，则本地分支和远程分支都可以省略。

> ```bash
> $ git push origin
> ```

上面命令表示，将当前分支推送到`origin`主机的对应分支。

如果当前分支只有一个追踪分支，那么主机名都可以省略。

> ```bash
> $ git push
> ```

如果当前分支与多个主机存在追踪关系，则可以使用`-u`选项指定一个默认主机，这样后面就可以不加任何参数使用`git push`。

> ```bash
> $ git push -u origin master
> ```

上面命令将本地的`master`分支推送到`origin`主机，同时指定`origin`为默认主机，后面就可以不加任何参数使用`git push`了。

不带任何参数的`git push`，默认只推送当前分支，这叫做simple方式。此外，还有一种matching方式，会推送所有有对应的远程分支的本地分支。Git 2.0版本之前，默认采用matching方法，现在改为默认采用simple方式。如果要修改这个设置，可以采用`git config`命令。

> ```bash
> $ git config --global push.default matching
> # 或者
> $ git config --global push.default simple
> ```

还有一种情况，就是不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机，这时需要使用`--all`选项。

> ```bash
> $ git push --all origin
> ```

上面命令表示，将所有本地分支都推送到`origin`主机。

如果远程主机的版本比本地版本更新，推送时Git会报错，要求先在本地做`git pull`合并差异，然后再推送到远程主机。这时，如果你一定要推送，可以使用`--force`选项。

> ```bash
> $ git push --force origin 
> ```

上面命令使用`--force`选项，结果导致远程主机上更新的版本被覆盖。除非你很确定要这样做，否则应该尽量避免使用`--force`选项。

最后，`git push`不会推送标签（tag），除非使用`--tags`选项。

> ```bash
> $ git push origin --tags
> ```



创建本地分支，切换分支可以使用如下几条命令

```bash
git branch [分支名] # 创建本地分支，执行后不会切换到新的分支
git checkout [分支名] # 切换分支
git checkout -b [分支名] # 创建本地分支，并自动切换到新分支
```

新创建的分支的版本号与当前所在分支的版本号一致，并且默认不会跟踪任何远程分支。

所以当将修改内容提交到版本区后，要进行推送操作时，需要写明主机名、本地分支名、远程分支名。

```bash
git push [主机名] [本地分支名]:[远程分支名]
git push [主机名] [本地分支名] # 本地分支名和远程分支名相同时，远程分支名可省略
```

执行 上述命令就可以将本地分支推送到远程仓库的分支中，如果远程分支不存在，会自动创建。

一般情况下会推送到远程仓库的同名分支，即冒号前后的分支名是相同的，这时``:[远程分支名]``可以省略 。

例如，下面的两条命令的效果是一样的。

> ```bash
> $ git push origin dev:dev
> $ git push origin dev
> ```

`git push`的默认行为

```bash
git push [主机名]  
git push
```

在[git的全局配置中，有一个push.default](http://git-scm.com/docs/git-config)属性，其决定了`git push`操作的默认行为。

**push.default** 有以下几个可选值：nothing, current, upstream, simple, matching.

其用途分别为：

- **nothing** - push操作无效，除非显式指定远程分支，例如`git push origin develop`
- **current** - push当前分支到远程同名分支，如果远程同名分支不存在则自动创建同名分支。
- **upstream** - push当前分支到它的upstream分支上（这一项其实用于经常从本地分支push/pull到同一远程仓库的情景，这种模式叫做central workflow）。
- **simple** - simple和upstream是相似的，只有一点不同，simple必须保证本地分支和它的远程
  upstream分支同名，否则会拒绝push操作。
- **matching** - push所有本地和远程两端都存在的同名分支。

在Git 2.0之前，这个属性的默认被设为'matching'，2.0之后则被更改为了'simple'。

我们可以通过`git version`确定当前的git版本，通过`git config --global push.default 'option'`改变push.default的默认行为（或者也可直接编辑~/.gitconfig文件）。

因此如果我们使用了git2.0之前的版本，push.default = matching，git push后则会推送当前分支代码到远程分支，而2.0之后，push.default = simple，如果没有指定当前分支的upstream分支，就会收到上文的fatal提示。

不过，根据实验，当本地分支没有对应的upstream分支时，使用``git push origin``,``git push``均会提示错误。

```bash
$ git push origin
fatal: The current branch 002 has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin 002
```

而如果这时push后面的远程主机名是其他的远程仓库，例如``git push coop``,则能够push成功。

```bash
$ git push coop
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 4 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (7/7), 1.01 KiB | 344.00 KiB/s, done.
Total 7 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), done.
remote:
remote: Create a pull request for '002' on GitHub by visiting:
remote:      https://github.com/Broduker/CoopTestRepo/pull/new/002
remote:
To github.com:Broduker/CoopTestRepo.git
 * [new branch]      002 -> 002
```

因为新创建的分支默认不会跟任何远程分支关联，所以为了可以直接使用``git push``命令,可以使用下面命令使本地分支跟踪远程分支 。

```bash
git branch -u [主机名/远程分支名] [本地分支名]
git branch -u [主机名/远程分支名] # 本地分支名为当前所在分支时，可省略
```

其中``-u``是``--set-upstream``的缩写，也可以使用 `--unset-upstream`  撤销本地分支对远程分支的跟踪。

```bash
git branch --unset-upstream [本地分支名]
git branch --unset-upstream # 本地分支名为当前所在分支时，可省略
```

下面两条命令的效果是相同的：

> ```bash
> $ git branch -u origin/dev dev
> $ git branch -u origin dev
> ```



远程分支的删除：

```bash
git push [主机名] --delete [远程分支名] 
git push [主机名] :[远程分支名] # 相当于上传空的本地分支到远程分支
git push [主机名] :[远程分支名] :[远程分支名] ... # 删除多个远程分支
```

本地分支的更名与删除

```bash
git branch -D [分支名] # 删除本地分支
git branch -D [分支名] [分支名] ... # 删除多个本地分支
git branch -m [原分支名] [新分支名] # 重命名本地分支
```

**注意当前所在分支不能删除**



## upstream & downstream

说到这里，需要解释一下[git中的upstream到底是什么](http://stackoverflow.com/questions/2739376/definition-of-downstream-and-upstream)：

> git中存在upstream和downstream，简言之，当我们把仓库A中某分支x的代码push到仓库B分支y，此时仓库B的这个分支y就叫做A中x分支的upstream，而x则被称作y的downstream，这是一个相对关系，每一个本地分支都相对地可以有一个远程的upstream分支（注意这个upstream分支可以不同名，但通常我们都会使用同名分支作为upstream）。

初次提交本地分支，例如`git push origin develop`操作，并不会定义当前本地分支的upstream分支，我们可以通过`git push --set-upstream origin develop`，关联本地develop分支的upstream分支，另一个更为简洁的方式是初次push时，加入-u参数，例如`git push -u origin develop`，这个操作在push的同时会指定当前分支的upstream。

注意push.default = current可以在远程同名分支不存在的情况下自动创建同名分支，有些时候这也是个极其方便的模式，比如初次push你可以直接输入 git push 而不必显示指定远程分支。

## git pull

弄清楚`git push`的默认行为后，再来看看`git pull`。

当我们未指定当前分支的upstream时，通常`git pull`操作会得到如下的提示：

```bash
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> new1
```

`git pull`的默认行为和`git push`完全不同。当我们执行`git pull`的时候，实际上是做了`git fetch + git merge`操作，fetch操作将会更新本地仓库的remote tracking，也就是refs/remotes中的代码，并不会对refs/heads中本地当前的代码造成影响。

当我们进行pull的第二个行为merge时，对git来说，如果我们没有设定当前分支的upstream，它并不知道我们要合并哪个分支到当前分支，所以我们需要通过下面的代码指定当前分支的upstream：

```bash
git branch --set-upstream-to=origin/<branch> develop
# 或者git push --set-upstream origin develop 
```

实际上，如果我们没有指定upstream，git在merge时会访问git config中当前分支(develop)merge的默认配置，我们可以通过配置下面的内容指定某个分支的默认merge操作

```json
[branch "develop"]
    remote = origin
    merge = refs/heads/develop // [1]为什么不是refs/remotes/develop?
```

或者通过command-line直接设置：

```bash
git config branch.develop.merge refs/heads/develop
```

这样当我们在develop分支git pull时，如果没有指定upstream分支，git将根据我们的config文件去`merge origin/develop`；如果指定了upstream分支，则会忽略config中的merge默认配置。

以上就是git push和git pull操作的全部默认行为，如有错误，欢迎斧正

------

[1] 为什么merge = refs/heads/develop 而不是refs/remotes/develop?
因为这里merge指代的是我们想要merge的远程分支，是remote上的refs/heads/develop，文中即是origin上的refs/heads/develop，这和我们在本地直接执行`git merge`是不同的(本地执行`git merge origin/develop`则是直接merge refs/remotes/develop)。



