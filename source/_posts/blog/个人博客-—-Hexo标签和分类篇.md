---
title: 个人博客 — Hexo标签和分类篇
date: 2018-11-09 13:16:18
tags:
- hexo
categories:
- Blog
---
# 标签 #

## 新建标签页面 ##

1. 在站点目录下，打开Git Bash命令窗口
2. 输入以下命令，新建标签页面，该命令将在.\source\tags目录下创建文件index.md  
```bash
hexo new page tags
```

## 设置页面类型 ##

1. 访问目录.\source\tags，并用记事本打开文件index.md
2. 添加 type: "tags", 将页面类型设置为tags
```
---
title: 标签
date: 2018-11-08 12:02:31
type: "tags"
---
```
<!-- more -->

## 修改菜单 ##

1. 打开Next配置文件_config.yml
2. 查找"Menu settings"，删除tags设置所在的#号
```
menu:

	home: /
	tags: /tags
```
3. 启用Tags以后，首页会显示“标签”的选项

## 为新建的博文添加标签 ##

1. 以下示例为博文添加了两个标签: Blog 和 Hexo 
```
---
title: 个人博客 —— Hexo标签和分类篇
date: 2018-11-09 13:16:18
tags:
- Blog
- Hexo
```
# 分类 #

## 新建分类页面 ##

1. 在站点目录下，打开Git Bash命令窗口
2. 输入以下命令，新建标签页面，该命令将在.\source\categories目录下创建文件index.md  
```
hexo new page categories
```
## 设置页面类型 ##

1. 访问目录.\source\categories，并用记事本打开文件index.md
2. 添加 type: "categories", 将页面类型设置为categories
```
---
title: 分类
date: 2018-11-08 12:02:31
type: "categories"
---
```
## 修改菜单 ##

1. 打开Next配置文件_config.yml
2. 查找"Menu settings"，删除categories设置所在的#号
```
menu:

	home: /
	categories: /categories
```
3. 启用categories以后，首页会显示“分类”的选项

## 为新建的博文添加标签 ##

1. 以下示例为博文添加了一个分类: 互联网 
```
---
title: 个人博客 —— Hexo标签和分类篇
date: 2018-11-09 13:16:18
tags:
- 互联网
```