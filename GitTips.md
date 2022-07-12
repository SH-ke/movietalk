# git 使用笔记

## 新建项目并上传

```shell
git init # 初始化
git config --global user.email
git config --global user.name
# 取消y
git config --global http.sslVerify "false"

git add README.md # 添加单个文件 【. 可以代表所有文件】
git commit -m "first commit"
git branch -M main # 添加至主分支
git remote add origin https://github.com/SH-ke/temp.git
git push -u origin main
```

## 查看配置

```shell
git config --global -l # 查看用户配置
```

## 分支

```shell
# 查看所有分支
git branch -a 
# 更换分支
git checkout origin/main 
# 恢复至上一次commit
git log
git reset --hard 2f0080513c3cf8d2d384970f87b3094577352b15
```

## Git 学习笔记 -- 基础命令

### help

显示帮助信息

```shell
# 常用命令以及简短解释
git
git help
# -a 显示所有命令
git help -a
# git 使用手册
git help -g
# 查看具体命令
# Linux 可以使用 F, B, Q 翻页、向上翻页、退出
git help add
```

### config

简单配置git，分为系统设置、用户设置、项目范围设置

```shell
# 再用户范围设置 global
# 添加用户名和email
git config --global user.name 'SH-ke'
git config --global user.email 'SHke_dev@163.com'

# 查看配置信息
git config --list

# 删除指定配置
git config --unset --gloabal user.name

# 查看git config 命令的帮助
git config --help

# 为git输出添加色彩高亮
git config --global color.ui true

# 查看 ~/.gitconfig 文件
# 用户范围的配置文件保存在当前用户的家目录下 .gitconfig 文件中
cat ~/.gitconfig
```

### init

新建一个 git 项目

```shell
# 新建一个 movietalk 项目
mkdir movietalk; cd movietalk
git init
# 查看 .git/ 文件夹的目录结构
    # config 文件保存项目级别的配置文件
    # 若想要解除git对项目的控制，删除 ./git 文件即可
cd .git; ls
```

### commit

进行一次提交

```shell
# 查看当前状态
git status
# Initial commit 表示初始提交
# nothing to commit 表示没有需要提交的文件

# 新建 *.html 文件并保存
New-Item index.html
# 再次查看状态
git status
# Untracked files 为追踪的文件

# 添加未跟踪的文件
git add index.html
# 使用 . 表示添加全部文件
git add .
# 再次查看状态
git status 
# Changes to be commited 以下修改将要被提交
# new file:    index.html 新建了新的文件

# 为提交添加备注
# 使用 -m 参数，添加注释，否则使用默认的编辑器 vim
git commit -m '添加 index.html 文件'
# 再次查看状态
git status 
# nothing to commit, working tree is clean

# 查看提交记录
git log
# 提交id author date desc
```

### diff

查看文件修改前后的区别

```shell
# 查看修改前后的区别 工作目录中的修改
git diff index.html
# 查看所有文件的区别
git diff
# 显示修改，删除一行 添加一行 并高亮显示

# 将修改提交至暂存区
git add index.html
# 再次使用 diff
git diff index.html
# 并无输出 说明工作目录中并无修改 保存到暂存区

# 不使用 commit 再次修改 index.html
git status
git diff index.html
# Changes to be commited 缓存区中未提交的修改
# Changes not staged for commit 工作目录中为保存到缓存区的修改

# 查看 缓存区 与 reposity 的区别
git diff --staged

# 将这两个修改分别提交至 reposity
git log
# 查看三个修改信息
```

工作目录修改 -> git add -> 暂存区 -> git commit -> reposity 仓库

### rename

重命名 git 跟踪的文件

```shell
# 新建 style.css 文件 并提交至 repo
# 手动修改文件名 style.css -> theme.css
git status 
# git 认为是删除了 style.css 同时 新建了 theme.css
git rm style.css
git add theme.css
git status 
# 状态信息显示了 renamed: style.css -> theme.css
# 将修改提交至 repo
```

### mv

移动文件、重命名

```shell
# 重命名文件 theme.css -> shke-theme.css
git mv theme.css shke-theme.css
git status
# 将修改提交至 repo

# 移动文件
mkdir css; git status
# git 仅跟踪文件、含有文件的目录 所以此处工作目录并无修改
# 移动文件
git mv ./shke-theme.css ./css/shke-theme.css 
git status

# 同样的操作 将 css 目录移动至 assert 目录下
```

### rm

```shell
# 删除命令
git rm ./assert/css/shke-theme.css
# 递归删除
git rm -r
# 使用 git 删除 该文件必须已经提交至 repo
# 若修改了该文件 同时未将其提交至 repo 则无法删除文件
```

### head

从历史纪录中恢复删除的文件

```shell
# 从暂存区恢复
git rm index.html # 删除文件
# 此时修改并未提交至 repo
git checkout HEAD -- index.html
# 将 index.html 文件 恢复

# 从 repo 恢复 index.html 至上一状态
# 删除 index.html 同时提交至 repo
# 使用 ^ 表示 将 index.html 文件恢复至最近一次提交的上一次提交，两个 ^ 则表示上两次提交
# 最近一次提交是 删除 index.html 文件，所以要恢复 inde.html 文件应当选择 最近一次提交的上一次提交
git checkout HEAD^ -- index.html
# status 显示 new file
# 提交至 repo
```

### revert

回复，清除某次操作

```shell
# 分别创建两次提交 为项目添加 BootStrap jQuery
# 查看历史记录 精简版
git log --oneline

# 回复
git revert cd2d265
# 仅删除 该处操作的修改 保留后续其他修改
# 但不能出现冲突
```

```html
<!-- title 之前 -->
<link rel="stylesheet" href="css/bootstrap.min.css">
<!-- body 标签内 -->
<script src="js/jquery-2.1.1.min.js"></script>
```

### reset

```shell
# 重置 HEAD 指针
git reset 6547c53
# reset HEAD-id 跳转至指定 HEAD
# reset 命令有三种模式 soft hard mixed 默认值为 mixed
git reset --hard 6547c53
```

构建一个测试环境，测试 soft hard mixed 三种模式的区别。

1. 添加 new_page.html 文件，删除 trash.html 文件，在 index.html 文件中添加一个 div 标签，删除一个 a 标签。使用 git add 命令，将本次修改提交至 暂存区

2. 添加 hello.py 文件，删除 useless_package.py 文件，在 index.html 文件中添加一个 h1 标签，删除一个 注释。此条作为工作目录的修改

3. 对以上测试环境分别使用 reset 的三种模式进行测试，使用 git status，观察工作目录、缓存区变化。

三种模式的共同点：

1. 重置之后所有的 HEAD 均存在，不会删除；即使是 git log 命令中不现实的 HEAD ，只要使用 HEAD id，仍旧可以跳转至该 HEAD 下

2. repo 的状态都是一致的，三种模式跳转后 repo 中的文件内容、结构均相同

3. 没有跟踪的文件是相同的，git 并不会修改未跟踪的文件，使用 rest 命令前后 Untracked 文件仍旧为 Untracked

不同点：

1. hard 模式，工作区、暂存区、repo 均会重置到目标 HEAD 执行 git commit 命令之后的状态。只有 Untracked 文件会保留，而且之前所有的 HEAD 节点都是存在的，可以通过 HEAD id 进行跳转，但 reset 前一刻的工作目录、暂存区内容将不复存在

2. soft 模式，工作区的所有文件与修改保留，与目标 HEAD 相比，新增的文件被放入暂存区显示为 new file；存在且被修改过的文件存在于工作目录，被标记为 modified；未跟踪的文件仍旧为 Untracked

3. mixed 模式，工作区的所有文件与修改保留，暂存区为空，与目标 HEAD 相比，新增的文件被标记为 Untracked；存在且被修改过的文件存在于工作目录，被标记为 modified；未跟踪的文件仍旧为 Untracked

```shell
# 命令变种 -- 清除当前工作区、暂存区
git reset --hard HEAD
# 暂存区回退
git reset HEAD
```

## Git 学习笔记 -- 分支命令

### branch

新建、查看、切换分支

```shell
# 新建分支
git branch mobile-feature
# 查看所有分支 * 表示当前分支
git branch
# 切换分支
git checkout mobile-feature
```

### checkout

切换分支，修改项目并显示分支

```shell
# 切换分支
git checkout master
# 查看分支详情
git log --oneline --decorate
# 查看所有分支详情
git log --oneline --decorate --all
# 貌似在此电脑中，--decorate 参数是默认的
```

### branch diff

查看分支的不同

```shell
# 查看所有不同
git diff master..mobile-feature
# 这里显示的是 master mobile-feature 这两个分支的所有区别

# 指定文件
git diff master..mobile-feature index.html
# 仅显示 index.html 文件的区别
```

### fast forward

平直合并，两个分支是前后关系，并无冲突，也无分歧，直接合并

```shell
# 查看分支
git branch
# 在 master 分支下执行 merge 命令
git merge mobile-feature

# 再次查看两个分支的区别 
git diff master..mobile-feature
```

### merge

分歧合并，两个分支分别存在不同的修改，但修改在不同的位置，不存在冲突

```shell
# 在 mobile-feature 分支中 为 index.html 添加 web app meta 标签
# 在 master 分支中添加 athors.txt 文件

# 使用 -am 参数可以合并 add -m 两个操作
git commit -am 'string'
# 指定显示条数 以图形式展示
git log --oneline --all -10 --graph

# 在 master 分支中执行 merge 命令
git merge mobile-feature
# 再次查看两分支的区别
git diff master..mobile-feature
```

添加 web app meta 标签

```html
<!-- title 标签之前 -->
<meta name="apple-mobile-web-app-capable"content="yes">
```

### conflict

解决合并冲突

```shell
# 在 master 分支下修改 index.html 中的 title 标签为 Movietalk
# 跳转至 mobile-feature 分支 修改 title 标签为 movie_talk
# 同时在 mobile-feature 分支下执行 merge 命令
git log --oneline -10 --all --graph
git merge master
# 存在冲突 使用 abort 命令可以取消此次合并
git merge --abort

# 手动在编辑器中解决冲突 在执行如下提交指令则可继续进行提交
git add .
git commit
# 不必使用 -m 参数 在提交信息中删除 conflict 字段即可
```

### rm branch

重命名分支、删除分支

```shell
# 新建一个分支 bugfix
git branch bugfix
# 查看所有分支 以下两个命令作用相同
git branch
git branch --list

# 将 bugfix 分支 重命名为 bugfix-1
git branch -m bugfix bugfix-1
# 查看所有分支
git branch 

# 删除分支 bugfix-1
git branch -d bugfix-1
# 查看所有分支
git branch
```

### stash

保存工作进度，但不干扰其他修改，此进度也不参与提交

```shell
# 保存进度
git stash save '修改了 authors.txt'
# 查看工作进度
git stash list
# 查看工作进度与工作区的区别
git stash show -p 'stash@{0}'
# -p 表示以补丁的方式查看

# 恢复工作进度
git stash apply 'stash@{0}'
# 删除工作进度
git stash drop 'stash@{0}'
# 再次查看 发现工作进度已删除
git stash list

# 获取最近一次的工作进度 恢复至工作目录 然后将其删除
git stash pop
```

### log

查看日志、选择展示形式、筛选

```json
// git log 长文本翻页
{
    'f/<Space>': '向下翻页', 
    'b': '向上翻页',
    'j': '向上滚轮',
    'k': '向下滚轮',
    'q': '退出',
}

// 展示形式
{
    '--oneline': '单行精简显示', 
    '-5': '指定输出总数', 
    '--author="SH-ke"': '指定提交作者', 
    '--graph': '添加图形展示', 
}

// 筛选日志
{
    '--grep="index.html"': '筛选提交信息中含有关键词 index.html 的 HEAD', 
    '--before="2022-7-1"': '筛选该日期之前的日志', 
    '--before="3 days"': '描述形式的事件表示 显示三天前的日志', 
}
```

### alias

为命令设置别名

```shell
# 在用户范围内设置别名
git config --global alias.co checkout
# 自定义设置
git config --global alias.gal 'log --oneline --all --graph -10' 
# 查看用户配置文件
cat ~/.gitconfig
```

### ignore

忽略目录，添加指定目录后 git 将不再跟踪该目录中的文件

```shell
# 用户级别的忽略
git config --global core.excludesfile ~/.gitignore_global
vim ~/.gitignore_global
# 在该文件中填写指定目录即可

# 项目级别的忽略
vim .gitignore

# 参考官网提供的 gitignore 模板
https://github.com/github/gitignore

# gitignore 并不会忽略已经跟踪的文件 因此要对指定文件取消跟踪
# 展示所有跟踪文件
首先 git rm -r -n --cached .
# 取消跟踪文件
git rm --cached revert_log.txt
# 取消跟踪目录 -r 表示递归
git rm -r --cached venv
```

忽略 *.log 文件

```ini
*.log
venv
```

## Git 学习笔记 -- 远程仓库

### origin

创建远程

```shell
# 在 github 上创建一个 repo 并复制地址
# 在本地 repo 中新建一个名为 origin 的远程
git remote add origin 'https://github.com/SH-ke/movietalk.git'
# 查看远程
git remote
# 查看远程详细信息 verbose adj.
git remote -v
# 移除远程 repo
git remote rm origin
```

```json
// git remote add 参数
{
    'origin': '远程的名称 常用 origin 表示 任意名称均可', 
    'https...': '远程地址', 
}
```

### push

将本地项目推送到远程 repo

```shell
# 将 master 分支推送到 origin 远程
# -u 表示 set-up-to-track-remote-branch..
# 本地的 master 分支将会跟踪远程仓库的 master 分支
git push -u origin master
# 此处会提示要输入用户名和密码
# Branch 'master' set up to track remote branch 'master' from 'origin'.

# 查看本地分支
git branch
# 查看所有分支
git branch -a
# 查看远程分支
git branch -r

# 推送 mobile-feature 分支到远程 不跟踪远程 repo
git push origin mobile-feature
```

### clone

从远程仓库克隆 repo

```shell
# 张三视角-克隆将项目重命名
git clone https://github.com/SH-ke/movietalk.git movietalk_zhang

# SH-ke 视角- 修改 author.txt 文件并上传远程 repo
git push origin master
```
