---
title: 个人博客 — Hexo多终端同步篇
date: 2019-01-01 09:45:38
tags:
- hexo
categories:
- Blog
---
如果需要在多台终端设备上发布博客，那么就需要将Hexo源代码发布到GitHub，并且在多台终端设备之间同步。

# 配置远程仓库 #
1. 在GitHub上创建一个新的分支，例如，分支名称为Hexo
![](21.png)

2. 将Hexo分支设置成默认分支
![](22.png)

<!-- more -->

# 配置本地仓库 #

1. 注意！！！如果使用git clone下载的三方主题，要把主题文件夹下面把.git文件夹删除掉，不然主题无法push到远程仓库，导致你发布的博客是一片空白

2. 在初次安装Hexo的设备上，初始化博客所在目录
```bash
git init
```
3. 添加本地所有文件到仓库
```bash
git add -A
```

4. 添加远程仓库  
	我执行该命令后，提示fatal: remote origin already exists.可以忽略该报错。
```bash
git remote add origin git@github.com:yourname/yourname.github.io.git
```

5. 添加本地仓库分支Hexo
```bash
git branch hexo
```
6. 将本地Hexo分支的文件强制推送到远程Hexo分支
```bash
git push origin hexo -f
```
7. 切换到Hexo分支
```bash
git checkout -b hexo
```
上传完成之后，就会拥有两个远程的分支：master和hexo，其中master是部署成博客的分支；hexo是我们可以clone到其他电脑或其他系统的hexo源文件的分支，而且我们已经将它设置成默认仓库。

# 多终端同步和发布博客 #

1. 在其它终端设备上clone远程分支hexo到本地
```bash
git clone -b hexo git@github.com:yourname/yourname.github.io.git
```
2. 进入本地仓库执行hexo安装
```bash
npm install
```
3. 先将远程分支同步到本地hexo分支
```bash
git pull origin hexo
```
4. 编辑本地博客后，同步到远程hexo分支
```bash
git add "blog files"
git commit -m "comments"
git push origin hexo
```
5. 发布博客
```bash
hexo g -d
```
