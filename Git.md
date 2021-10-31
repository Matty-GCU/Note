# Git复习笔记

## 前言

### **基于**

[廖雪峰的官方网站 - Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)

### **简介***

*（详见原教程）*

* Git的诞生
* 集中式vs分布式

## 基本知识

### Git初次操作*

**安装Git**

*（详见原教程）*

**创建版本库**

版本库又名**仓库（repository）**，该目录下的所有文件都可以由Git管理	

1.选择合适的地方创建一个空目录（右键菜单 - Git Bash Here）

```
$ mkdir learngit	创建空目录
$ cd learngit		进入空目录
$ pwd				显示当前目录
```

2.把这个目录变成Git可以管理的仓库

```
$ git init
```

### 添加、提交操作

```
$ git add readme.txt						把文件添加到仓库
$ git commit -m "wrote a readme file"		把文件提交到仓库(-m后面输入的是本次提交的说明)
```

* 所有的版本控制系统都只能跟踪**纯文本文件**的改动（判断方法：用记事本打开之后会不会显示乱码）
* 执行`git add`命令没有任何显示，这就对了，因为Unix的哲学是**“没有消息就是好消息”**。

### 工作区、暂存区、版本库

**工作区（Working Directory）**

就是你在电脑里能直接看到的、被Git管理的目录，比如`learngit`文件夹就是一个工作区。

**版本库（Repository）**

工作区的隐藏目录`git`，就是Git的版本库。

**暂存区（stage）**

Git的版本库里存着被称为`stage`的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

* 需要提交的文件修改通通放到暂存区，然后一次性提交暂存区的所有修改。

* 一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是干净（`clear`）的。

![](https://www.liaoxuefeng.com/files/attachments/919020037470528/0)

## 时光机穿梭

### 管理修改

**Git跟踪并管理的是修改，而非文件。**

操作：第一次修改 -> `git add` -> 第二次修改 -> `git commit`

结果：第二次的修改没有被提交

```
$ git diff HEAD -- readme.txt		查看工作区中该文件和版本库里面最新版本的区别
```

### 版本回退★

**使用`git status`命令，随时掌握工作区的状态**

```
$ git status
$ git diff readme.txt	查看修改内容
```

**使用`git log`命令，显示当前分支所有提交过的版本信息**

```
$ git log
$ git log --pretty=oneline	加上该参数提高可读性（注意是oneline不是online）
```

* 使用git log后将会看到一大串类似`1094adb...`的**版本号（commit id）**。

**使用`git reflog`命令，查看所有分支的所有操作记录，包括已经被删除的 commit 记录和 reset 的操作**

```
$ git reflog
```

**使用`git reset`命令，将当前版本回退到指定版本**

```
$ git reset --hard HEAD^
$ git reset --hard HEAD~1
$ git reset --hard 1094a
```

* Git的版本回退速度非常快，因为当你回退版本的时候，Git仅仅是把指向当前版本的`HEAD`指针从指向A改为指向B，然后顺便把工作区的文件更新了。也就是说，你让`HEAD`指向哪个版本号，你就把当前版本定位在哪。
* 在Git中，用`HEAD`表示当前版本，也就是最新的提交。上一个版本是`HEAD^`，上上一个版本就是`HEAD^^`，往上100个版本写成`HEAD~100`

### 撤销修改

**场景1：当你想直接丢弃工作区的修改时，用命令`git checkout -- <file>`**

```
$ git checkout -- readme.txt		丢弃工作区中该文件的修改，并没有改变暂存区的状态
```

* 若`readme.txt`自修改后还没有被添加到暂存区，那么撤销修改就回到和版本库一模一样的状态。

* 若`readme.txt`已经被添加到暂存区后，但又做了修改，那么撤销修改就回到添加到暂存区时的状态。

**场景2：当你已经把修改添加到了暂存区，再想丢弃修改时，用命令`git reset HEAD <file>`回到场景1**

```
$ git reset HEAD readme.txt			把暂存区的修改撤销掉（unstage），暂存区的状态变为clear
```

* 该命令也可以用来回退到指定版本（见上一节）。

### 删除文件

在Git中，删除也是一种修改操作。

```
$ git rm readme.text 或 $ git add readme.txt
$ git commit -m "remove readme.txt"
```

## 远程仓库

### GitHub初次操作*

*（详见原教程）*

* 注册GitHub
* 创建SSH Key
* 为GitHub账号添加SSH Key

### 删除仓库

**删除本地仓库**

操作：删除Git本地仓库根目录下的隐藏文件夹`.git`。

检验：在Gitbash中进入本地仓库目录，如果目录末尾没有`(master)`，说明成功删除。

**删除远程仓库**

在GitHub的对应仓库中打开Settings，滑到Setting页面的底部，找到Danger Zone的最后一个子项Delete this reposiory。

### 关联远程库

**创建**

GitHub  -->  右上角”+“  -->  New Repository  -->  输入Repository name  -->  其他保持默认设置

**关联**

```
$ git remote add origin git@github.com:Matty-GCU/learngit.git		这里远程库的名字是origin
$ git push -u origin master											第一次推送master分支应加上-u参数
$ git push origin master											把当前分支master推送到远程
```

* 关联一个远程库时需要给远程库指定一个名字，`origin`是默认习惯命名。
* 每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改。

**解除绑定**

```
$ git remote -v														查看远程库信息
$ git remote rm origin												根据名字删除远程库
```

### 从远程库克隆

用命令`git clone`从远程库克隆一个本地库

```
$ git clone git@github.com:Matty-GCU/gitskills.git
$ git clone https://github.com/Matty-GCU/gitskills.git
```

* Git支持多种协议，包括`https`，但`ssh`协议速度最快、最方便。

## 分支管理

### 创建与合并分支

**常用命令**

```
$ git branch									查看所有分支（当前分支前面会标一个*号）
$ git branch dev								创建新分支
$ git switch dev								切换分支
$ git switch -c dev								创建并切换分支
$ git merge dev									合并指定分支到当前分支
$ git branch -d dev								删除分支
```

* 严格来说，`HEAD`指向的不是提交，而是`master`；`master`才是指向提交的。
* 因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支。

### 解决冲突

如果分支`master`和分支`dev`都有各自独立的修改，则无法进行”快速合并“`fast forward`。

在这种情况下`git merge master`，Git会告诉我们，该文件存在冲突`conflicts`，需要手动解决冲突。

`git status`可以告诉我们冲突的文件；

此时直接查看文件内容，会看到Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容。

手动修改后保存，提交，删除次分支，OK。
