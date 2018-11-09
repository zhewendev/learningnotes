# 2. git初始

## 2.2 git暂存区

在版本库`.git`目录下，有一个`index`文件

当执行**git status**命令（或者**git diff**命令）扫描工作区改动的时候，先依据`.git/index`文件中记录的（工作区跟踪文件的）**时间戳、长度等信息判断工作区文件是否改变**。如果工作区的文件时间戳改变，说明文件的内容**可能**被改变了，需要要打开文件，读取文件内容，和更改前的原始文件相比较，判断文件内容是否被更改。如果文件内容没有改变，则将该文件新的时间戳记录到`.git/index`文件中。因为判断文件是否更改，使用时间戳、文件长度等信息进行比较要比通过文件内容比较要快的多，所以Git这样的实现方式可以让工作区状态扫描更快速的执行，这也是Git高效的因素之一

文件`.git/index`实际上就是一个包含文件索引的目录树，像是一个虚拟的工作区。在这个虚拟工作区的目录树中，记录了文件名、文件的状态信息（时间戳、文件长度等）。文件的内容并不存储其中，而是保存在Git对象库`.git/objects`目录中，文件索引建立了文件和对象库中对象实体之间的对应。下面这个图展示了工作区、版本库中的暂存区和版本库之间的关系。

![](http://www.worldhello.net/gotgit/images/git-stage.png)

- 图中左侧为工作区，右侧为版本库。在版本库中标记为`index`的区域是暂存区（stage，亦称index），标记为`master`的是master分支所代表的目录树。
- 图中可以看出此时HEAD实际是指向master分支的一个“游标”。所以图示的命令中出现HEAD的地方可以用master来替换。
- 图中的objects标识的区域为Git的对象库，实际位于`.git/objects`目录下
- 当对工作区修改（或新增）的文件执行**git add**命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID被记录在暂存区的文件索引中。
- 当执行提交操作（**git commit**）时，暂存区的目录树写到版本库（对象库）中，master分支会做相应的更新。即master最新指向的目录树就是提交时原暂存区的目录树。
- 当执行**git reset HEAD**命令时，暂存区的目录树会被重写，被master分支指向的目录树所替换，但是工作区不受影响。
- 当执行**`git rm –cached <file>`**命令时，会直接从暂存区删除文件，工作区则不做出改变。
- 当执行**git checkout .**或者**git checkout – <file>**命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区的改动。
- 当执行**git checkout HEAD .**或者**git checkout HEAD <file>**命令时，会用HEAD指向的master分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。

## **暂存区目录树的浏览**

对于HEAD（版本库中当前提交）指向的目录树，可以使用Git底层命令**git ls-tree**来查看

```shell
$ git ls-tree -l head
100644 blob fd3c069c1de4f4bc9b15940f490aeb48852f3c42      25    readme.md
```

- 使用`-l`参数，可以显示文件的大小。上面`welcome.txt`大小为25字节。
- 输出的`welcome.txt`文件条目从左至右，第一个字段是文件的属性(rw-r–r–)，第二个字段说明是Git对象库中的一个blob对象（文件），第三个字段则是该文件在对象库中对应的ID——一个40位的SHA1哈希值格式的ID（这个会在后面介绍），第四个字段是文件大小，第五个字段是文件名

浏览暂存区中的目录树之前，首先清除工作区当中的改动。通过**git clean -fd**命令清除当前工作区中没有加入版本库的文件和目录（非跟踪文件和目录），然后执行**git checkout .**命令，用暂存区内容刷新工作区。

```shell
git clean -fd
git checkout .
```



要显示暂存区的目录树，可以使用**git ls-files**命令。

```shell
git ls-files -s
```

通过系列操作使得工作区，暂存区，head区三个目录树的内容各不相同，通过不同参数的git diff命令对其两两比较。

![](http://www.worldhello.net/gotgit/images/git-diff.png)

[具体流程参见](http://www.worldhello.net/gotgit/02-git-solo/020-git-stage.html)

**不要使用`git commit -a**`

该命令是对本地所有变更的文件执行提交操作，包括本地修改的文件，删除的文件，但不包括未被版本库跟踪的文件。这个命令丢掉了Git暂存区对提交内容进行控制的能力。



## git对象

### git对象库

通过查看日志的详尽输出，会惊讶的看到非常多的“魔幻数字”，这些“魔幻数字”实际上是SHA1哈希值。

```shell
git log -1 --pretty=raw
```

研究Git对象ID的一个重量级武器就是**git cat-file**命令。用下面的命令可以查看一下这三个ID的类型。

```shell
git cat-file -t e1486e74
```

引用对象ID，只需从头开始的几位不冲突即可

![](http://www.worldhello.net/gotgit/images/git-objects.png)

![](http://www.worldhello.net/gotgit/images/git-repos-detail.png)

原来分支master指向的是一个提交ID（最新提交）。这样的分支实现是多么的巧妙啊：既然可以从任何提交开始建立一条历史跟踪链，那么用一个文件指向这个链条的最新提交，那么这个文件就可以用于追踪整个提交历史了。这个文件就是`.git/refs/heads/master`文件。

目录`.git/refs`是保存引用的命名空间，其中`.git/refs/heads`目录下的引用又称为分支。对于分支既可以使用正规的长格式的表示法，如`refs/heads/master`，也可以去掉前面的两级目录用`master`来表示。Git 有一个底层命令**git rev-parse**可以用于显示引用对应的提交ID。

### SHAI哈希值是什么？如何生成

哈希(hash)是一种数据摘要算法（或称散列算法），是信息安全领域当中重要的理论基石。该算法将任意长度的输入经过散列运算转换为固定长度的输出。固定长度的输出可以称为对应的输入的数字摘要或哈希值。例如SHA1摘要算法可以处理从零到一千多万个TB的输入数据，输出为固定的160比特的数字摘要。两个不同内容的输入即使数据量非常大、差异非常小，两者的哈希值也会显著不同。比较著名的摘要算法有：MD5和SHA1。Linux下**sha1sum**命令可以用于生成摘要。

Git中的各种对象：提交，文件内容，目录树等对应的SHAI哈希值生成过程：

[操作流程](http://www.worldhello.net/gotgit/02-git-solo/030-head-master-commit-refs.html)

**为什么不用顺序数字表示提交**？

因为分布式，开发非线性，需要做到全球唯一

## git重置

### 分支游标master

引用`refs/heads/master`就好像是一个游标，在有新的提交发生的时候指向了新的提交。可是如果只可上、不可下，就不能称为“游标”。Git提供了**git reset**命令，可以将“游标”指向任意一个存在的提交ID。下面的示例就尝试人为的更改游标。命令中使用了`--hard`参数，会破坏工作区未提交的改动，慎用。

```shell
git reset --hard head^
```

这条命令就相当于将master重置到上一个老的提交上.之前的文件相关操作也丢失了

可以通过提交ID重置到任何一次提交。

```shell
git reset --hard e1486e7
```

使用重置命令，会彻底丢弃历史，让提交历史也改变了，不能够通过浏览提交历史找到丢弃的提交ID，在重置恢复。

### 用reflog挽救重置

Git提供了一个挽救机制，通过`.git/logs`目录下日志文件记录了分支的变更。

git提供了一个`git reflog`命令，

```shell
git reflog show master | head -5
```

或`git reflog`

接着通过reset命令重置回来

```shell
git reset --hard e1486e7
```



`--hard`是git reset命令的可选参数，其他用法参见[权威指南](http://www.worldhello.net/gotgit/02-git-solo/040-git-reset.html)或者git help reset查阅帮助

## Git检出

HEAD可以理解为“头指针”，是当前工作区的“基础版本”，当执行提交时，HEAD指向的提交将作为新提交的父提交。看看当前HEAD的指向。

“分离头指针”状态指的就是HEAD头指针指向了一个具体的提交ID，而不是一个引用（分支）。

