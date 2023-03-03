---
title: Pelican小贴士
date: 2021-9-20 16:21:00
tags: 
 - pelican
categories: 
 - Blog
---

## 如何修改每篇博文的作者、分类、日期等信息

修改文件`article_infos.html`，文件位置：`~/blog/venv/lib/python3.7/site-packages/pelican/themes/notmyidea/templates`

以下内容中，已将`author`和`category`的相关代码注释，所以博文中将不会出现作者和分类的信息。

```html
<footer class="post-info">
        <abbr class="published" title="{{ article.date.isoformat() }}">
                日期: {{ article.locale_date }}
        </abbr>
        {% if article.modified %}
        <!--            <br /> -->
        <abbr class="modified" title="{{ article.modified.isoformat() }}">
                更新: {{ article.locale_modified }}
        </abbr>
        {% endif %}

<!--        {% if article.authors %}
        <address class="vcard author">
                作者: {% for author in article.authors %}
                        <a class="url fn" href="{{ SITEURL }}/{{ author.url }}">{{ author }}</a>
                {% endfor %}
        </address>
        {% endif %}
        <p>分类: <a href="{{ SITEURL }}/{{ article.category.url }}">{{ article.category }}</a>.</p> -->
{% include 'taglist.html' %}
{% import 'translations.html' as translations with context %}
{{ translations.translations_for(article) }}
</footer><!-- /.post-info -->
```

修改文件`main.css`，找到`.post-info`，修改以下内容后，字体大小和颜色，以及位置都会发生变化。

```css
.post-info {
    font-size:0.5em;
    color:#808080;
    float:top;
    margin:0;
    padding:0;
}
```

<!-- more -->

***

## 如何修改或者删除网页的脚注(Footer)

修改文件`base.html`，如果要删除脚注，只需要注释(`<!-- ... -->`)以下代码。

```HTML
<!-- <footer id="contentinfo" class="body">
                <address id="about" class="vcard body">
                Proudly powered by <a href="https://getpelican.com/">Pelican</a>, which takes great advantage of <a href="https://www.python.org/">Python</a>.
                </address>

                <p>The theme is by <a href="https://www.smashingmagazine.com/2009/08/designing-a-html-5-layout-from-scratch/">Smashing Magazine</a>, thanks!</p>
        </footer> --> <!-- /#contentinfo -->
```



***

## 如何修改导航栏的颜色和超链接的文字颜色

将导航栏的颜色修改为蓝色，可以修改`main.css`文件，找到`#banner nav`，然后将`background`设置为`#5faee3`。

```CSS
#banner nav {
                background: #5faee3;
                font-size: 1.143em;
                height: 40px;
                ...
}
```

将超链接文字的颜色修改为蓝色，可以修改`main.css`文件，找到`a:link, a:visited`，然后将`color`设置为`#5faee3`。

```css
a:link, a:visited {
        color: #5faee3;
        padding: 0 1px;
        text-decoration: underline;
}
```



## 如何让图片自动适应页面的宽度

以前，每次用手机拍摄的高分辨率图片插入博文后，图片都会以原始大小的长宽显示，导致无法在网页正常显示图片。

通过修改主题的`main.css`文件，并增加以下内容后，可以解决该问题。该方案使用的技术仍然是CSS响应式设计。

```css
img {
    max-width: 100%;
    height: auto;
}
```

**[参考资料]**

[响应式网页设计 - 图像 (w3school.com.cn)](https://www.w3school.com.cn/css/css_rwd_images.asp)

***

## 如何适应手机的屏幕

博客的默认主题notmyidear并不能够自动适应手机屏幕的大小，也就是自动根据屏幕调整字体和图片的大小。

通过查询相关资料，了解到了CSS响应式设计，即网页可以根据电脑、平板和手机的尺寸变化，自动调整大小、内容布局等。

首先，需要修改文件`base.html`，文件位置`~/blog/venv/lib/python3.7/site-packages/pelican/themes/notmyidea/templates`。在文件中，需要添加内容：`<meta name="viewport" content="width=device-width, initial-scale=1.0">`。

```html
<head>
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        {% block head %}
        <meta charset="utf-8" />
        <meta name="generator" content="Pelican" />
        ...
</head>
```

然后，修改主题的`main.css`文件，并增加以下内容后，移动设备的宽度为760px或以下，则会自动适配该CSS的定义。

```css
/*
 For mobile devices
 ***********************/

@media only screen and (max-width: 768px) {

  #content {
    width: 86%;
    font-size: 108%;
  }									//博客首页
  #featured {
    width: 86%;
    font-size: 108%;
  }									//博客背景
  #banner nav {
    width: 95%;
    font-size: 70%;
  }									//导航条
  #extras {
    width: 86%;
  }								    //其它博客
  .body {
    width: 100%;
  }									//单篇博客的内容
}
```

**[参考资料]**

[响应式网页设计 - 简介 (w3school.com.cn)](https://www.w3school.com.cn/css/css_rwd_intro.asp)

***

## 如何修改博客标题的字体类型

通过修改主题的`main.css`文件，设置需要的字体类型。例如，微软雅黑。

```bash
cd ~/blog/venv/lib/python3.7/site-packages/pelican/themes/notmyidea/static/css
vi main.css
```

打开main.css以后，查找关键字**font-family**，然后找到h1, h2, h3, h4, h5, h6的位置，添加**'Microsoft YaHei'**。

```css
h1, h2, h3, h4, h5, h6 {
        font-weight: 400;
        line-height: 1.1;
        margin-bottom: .8em;
    font-family: 'Microsoft YaHei', 'Yanone Kaffeesatz', arial, serif;
}
```

***

## 如何为文章创建目录大纲

修改Pelican配置文件`pelicanconf.py`，在配置文件中添加以下内容：

```python
# Support for Markdown Toc
MARKDOWN = {
    "extension_configs": {
        # Needed for code syntax highlighting
        "markdown.extensions.codehilite": {"css_class": "highlight"},
        "markdown.extensions.extra": {},
        "markdown.extensions.meta": {},
        # This is for enabling the TOC generation
        "markdown.extensions.toc": {"title": "目录"},
    },
    "output_format": "html5",
}
```

[参考资料]

- [Add a Table of Contents using Markdown in Pelican (cloudbytes.dev)](https://cloudbytes.dev/articles/add-a-table-of-contents-using-markdown-in-pelican)

