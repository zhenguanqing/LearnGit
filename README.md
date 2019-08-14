### Git介绍

Git是目前最先进的分布式版本控制系统

那什么是版本控制系统？

>如果有一个软件，不但能自动帮我记录每次文件的改动，还可以让同事协作编辑，这样就不用自己管理一堆类似的文件了，也不需要把文件传来传去。如果想查看某次改动，只需要在软件里瞄一眼就可以

这就是git的初衷

#### 集中式 vs 分布式

集中式版本控制系统是将版本库存放在中央服务器，每次干活之前和提交修改之后都要和中央服务器同步数据，所以必须联网才能工作，受网速限制。而且如果中央服务器坏了，版本库就没了

分布式版本控制系统没有中央服务器，每个人的电脑上都有完整的版本库，不需要联网就能工作（但是会有一台电脑充当“中央服务器”，仅仅是用来方便交换大家的修改），也不用担心某个版本库出问题。分步式系统更强大的地方还在于它的分支管理

### 安装Git

- 安装homebrew 然后通过homebrew安装Git [文档](http://brew.sh)
- 安装Xcode，然后运行Xcode 选择菜单“Xcode - Preferences - Downloads - Command Line Tools”

### 创建版本库

任意一个位置执行：

```
$ mkdir learngit
```
learngit文件夹用来存放git仓库

通过`git init` 把这个目录变成Git可以管理的仓库

```
$ git init
Initialized empty Git repository in /Users/zhenguanqing/learngit/.git/
```
此时这个文件夹就是一个空的git仓库 文件夹下会多一个.git 隐藏文件，里面保存的是各种仓库信息，不要随便改动

版本控制系统只能跟踪文本文件的改动，比如TXT，代码等，无法跟踪二进制文件的改动，比如图片、视频、Word文件

#### 对新建文件的操作

将新建的某个文件添加到仓库：
```
$ git add xxx
```
xxx代表添加的文件名


将所有新建的文件添加到仓库
```
$ git add .
```

将文件提交到仓库

```
$ git commit -m "xxxxx"
```
-m后边 双引号中的内容为提交的注释 会展现在历史记录中

`commit`命令可以一次提交很多文件，所以你可以多次add不同的文件之后再执行一次`commit`命令提交所有文件

### 远程仓库

#### 关联远程仓库

在GitHub（或其他的代码托管平台）创建一个仓库，然后将本地的仓库与GitHub上的仓库关联

```
$ git remote add origin git@github.com:xxx/xxx.git
```

add origin后面的是GitHub上的仓库地址，可以使HTTPS协议或者git协议（需要在代码托管平台添加自己电脑的SSH Key，方法很简单）

然后将本地仓库所有的内容推送到远程仓库上
```
$ git push -u origin master
```
如果出现类似下方这样的提示，表示关联成功了
```
Enumerating objects: 73, done.
Counting objects: 100% (73/73), done.
Delta compression using up to 4 threads
Compressing objects: 100% (62/62), done.
Writing objects: 100% (72/72), 690.35 KiB | 1.98 MiB/s, done.
Total 72 (delta 12), reused 0 (delta 0)
remote: Resolving deltas: 100% (12/12), done.
To github.com:zhenguanqing/DataStructureAndAlgorithm.git
   04a7d8a..81fe63e  master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

注：开始这一步总是失败，原因是创建的远程仓库必须为一个纯空仓库，任何文件都不能有，否则git会认为这是两个分支，解决方法是先把远程仓库的内容更新到本地：

```
git pull origin master
```
如果报错`fatal: refusing to merge unrelated histories`
就用
```
git pull --allow-unrelated-histories origin  master
```
`--allow-unrelated-histories`会允许关联两个分支的历史分支

#### 从远程仓库克隆

关联远程仓库还可以先创建远程库，然后克隆到本地，这种方式更常使用，因为有可能你是从半路开始开发这个项目的，此时这个项目已经存在了，当然也可以先在代码托管平台上创建远程仓库然后克隆到本地从零开始开发

此时拿到远程仓库地址后使用命令`git clone`克隆一个仓库（在任意一个文件夹目录下执行都可以，克隆成功后本地仓库就存在于这个文件夹下）
```
$ git clone git@github.com:xxx/xxx.git
```

例如：
```
$ git clone git@github.com:zhenguanqing/DataStructureAndAlgorithm.git
Cloning into 'DataStructureAndAlgorithm'...
remote: Enumerating objects: 79, done.
remote: Counting objects: 100% (79/79), done.
remote: Compressing objects: 100% (56/56), done.
remote: Total 79 (delta 15), reused 72 (delta 12), pack-reused 0
Receiving objects: 100% (79/79), 691.97 KiB | 117.00 KiB/s, done.
Resolving deltas: 100% (15/15), done.
```

### 版本与修改

#### 版本回退

随着在工作中对文件不停的修改，然后提交修改到版本库中，版本库会存在很多个`commit`，每次提交生成一个，可以把每次阶段性的修改放到一个`commit`中，如果出现错误，就可以从最近的一个`commit`恢复

通过 `git log`可以查看我们提交修改的历史记录（从近到远），加上`--pretty=oneline`可以展示精简信息

历时记录信息中类似 `1094adb...` 的是`commit id`（版本号）

如果回退到上一个版本？


