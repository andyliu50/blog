---
title: 个人博客 — Hexo安装篇
date: 2018-11-07 13:47:15
tags:
- hexo
categories:
- Blog
---
# 个人博客 — Hexo安装篇

## 什么是Hexo #

[Hexo](https://hexo.io/)是一个快速、 简单和强大的博客框架。你可以用Markdown或者其它语言撰写博客内容，Hexo会自动生成静态文件。

## 安装Node.js

**Node.js** 是一个基于 Chrome V8 引擎的 服务器端的JavaScript 运行环境。首先，需要下载并安装[Node.js](https://nodejs.org/en/)，然后再安装`Hexo`以及相关插件。

<!-- more -->

### Windows

1. 下载并安装[Node.js](https://nodejs.org/en/)。	 
2. 下载并安装[Git](https://git-scm.com/download/win)。 

### 树莓派

在树莓派上，先创建一个目录，用来存放`Node.js`文件，然后把下载的`Node.js`安装文件解压缩到目录`nodejs`下面。

下面以安装文件`node-v16.14.0-linux-armv7l.tar.xz`为例：

```bash
sudo mkdir -p /usr/local/lib/nodejs
sudo tar -xJvf node-v16.14.0-linux-armv7l.tar.xz -C /usr/local/lib/nodejs 
```

然后，在当前用户的配置文件`~/.profile`中，设置`Node.js`的环境变量：

```profile
export PATH=/usr/local/lib/nodejs/node-v16.14.0-linux-armv7l/bin:$PATH
```

刷新配置文件：

```bash
. ~/.profile
```

完成以上步骤后，`Node.js`已经安装成功。

通过以下命令，可以查看`node`, `npm`和`npx`的版本。

```bash
$ node -v
v16.14.0

$ npm -v
8.3.1

$ npx -v
8.3.1
```

通过修改配置文件`~/.profile`设置`Node.js`的方式，只能让当前用户能够使用最新版的node, npm和npx（其它用户，如root等无法使用新版`Node.js`）。而且在某些情况下，例如用crontab配置计划任务，或者将脚本配置为systemd服务的时候，仍然会使用旧版的`Node.js`。

为了解决以上问题，可以删除或者重命名`/usr/bin/`目录下的`node`，`npm`和`npx`，然后为新版的`Node.js`创建软链接：

```bash
sudo mv /usr/bin/node /usr/bin/node.bak
sudo mv /usr/bin/npm /usr/bin/npm.bak
sudo mv /usr/bin/npx /usr/bin/npx.bak
sudo ln -s /usr/local/lib/nodejs/node-v16.14.0-linux-armv7l/bin/node /usr/bin/node
sudo ln -s /usr/local/lib/nodejs/node-v16.14.0-linux-armv7l/bin/npm /usr/bin/npm
sudo ln -s /usr/local/lib/nodejs/node-v16.14.0-linux-armv7l/bin/npx /usr/bin/npx
```

## 安装Hexo #

在命令行中，运行以下命令，安装`Hexo`：

```Command
npm install -g hexo-cli 
```

验证`Hexo`安装成功。输入以下命令，可以看到`Hexo`的版本信息。
```Command
[Computer]:> hexo -v
INFO  Validating config
hexo: 6.0.0
hexo-cli: 4.3.0
os: linux 5.4.51-v7l+ Raspbian GNU/Linux 10 (buster) 10 (buster)
node: 16.14.0
v8: 9.4.146.24-node.20
uv: 1.43.0
zlib: 1.2.11
brotli: 1.0.9
ares: 1.18.1
modules: 93
nghttp2: 1.45.1
napi: 8
llhttp: 6.0.4
openssl: 1.1.1m+quic
cldr: 40.0
icu: 70.1
tz: 2021a3
unicode: 14.0
ngtcp2: 0.1.0-DEV
nghttp3: 0.1.0-DEV
```

## 安装Hexo插件

如果需要将博客部署到`Github Page`，还需要安装插件`hexo-deployer-git`：

```bash
npm install hexo-deployer-git --save
```

插件hexo-permalink-pinyin可以将博文链接中的中文字符转换为拼音：

```bash
npm i hexo-permalink-pinyin --save
```

## 配置Hexo

创建目录`blog`，并完成初始化：

```Command
[Computer]:> mkdir blog
[Computer]:> hexo init blog
INFO  Cloning hexo-starter https://github.com/hexojs/hexo-starter.git
INFO  Install dependencies
INFO  Start blogging with Hexo!
```

设置语言， 用记事本打开`_config.yml`， 设置`language`参数：

```yaml
language: zh-CN
```

修改主题：

```Command
git clone https://github.com/theme-next/hexo-theme-next themes/next  
```

 用记事本打开`_config.yml`， 设置`theme`参数。

```yaml
theme: next  
```

## 运行和访问

在命令行窗口中，运行命令:

```Command
[Computer]:> hexo s
INFO  Validating config
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/ . Press Ctrl+C to stop. 
```

 打开浏览器，输入URL: http://localhost:4000
 ![](08.png)