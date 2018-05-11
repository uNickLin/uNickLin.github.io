---
title: 談談 Hexo 怎麼配置
visible: true
date: 2018-05-10 14:52:29
tags: 
- markdown
categories: 
- Note
---

一開始接觸 Hexo 前就有聽聞他的支援套件很豐富，起手第一件就是先找個順眼的 theme，但因為每個主題提供的客製化程度都不一樣，所以挑了一圈最後還是選擇大眾口味的 Next，以下就大至分享我自己的 Next 設定以及整理一些解坑的方法。

<!--more-->

在開始前有兩個名稱相同，位置設定卻大不同的檔案要分清楚：
1. <font color='#156c08' style='font-weight: bold'>Next 配置檔</font>：/themes/_config.yml
2. <font color='#780909' style='font-weight: bold'>Hexo 配置檔</font>：/_config.yml

有些配置會在這兩個 config 間設定，就暫且叫他們 <font style='color: white; background-color: #156c08; padding: 0 5px; border-radius: 3px;'>Next config</font> <font style='color: white; background-color: #780909; padding: 0 5px; border-radius: 3px;'>Hexo config</font> 不然很容易搞混 ... 


## 安裝 Next

首先當然要先安裝 next 本人，到 hexo 資料夾下指令
```
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```
再來找到 <font style='color: white; background-color: #780909; padding: 0 5px; border-radius: 3px;'>Hexo config</font> 裡面的 theme 改成 next
```yml
theme: next
```
<small>[來源參照(大部分設定都可以在這找到)](https://theme-next.iissnan.com/getting-started.html#theme-settings)</small>


## 設定 Navbar
![Navbar](https://i.imgur.com/tgIwioK.png)

Navbar 在一開始預設只有兩個，分別是首頁 (home) 跟歸檔 (archive)，首先在 <font style='color: white; background-color: #156c08; padding: 0 5px; border-radius: 3px;'>Next config</font> 找到下面這段

```yml
# ---------------------------------------------------------------
# Menu Settings
# ---------------------------------------------------------------

# When running the site in a subdirectory (e.g. domain.tld/blog), remove the leading slash from link value (/archives -> archives).
# Usage: `Key: /link/ || icon`
# Key is the name of menu item. If translate for this menu will find in languages - this translate will be loaded; if not - Key name will be used. Key is case-senstive.
# Value before `||` delimeter is the target link.
# Value after `||` delimeter is the name of FontAwesome icon. If icon (with or without delimeter) is not specified, question icon will be loaded.
menu:
  home: / || home
  #about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive
  #schedule: /schedule/ || calendar
  #sitemap: /sitemap.xml || sitemap
  #commonweal: /404/ || heartbeat
```

把 tags 跟 categories 註解拿掉就會顯示在 Navbar 上，但這邊只是「顯示」，點進去還沒有頁面可以呈現，接著在 terminal 下

```
$ hexo new page categories
```

建立成功後會告訴你路徑位置

```
INFO  Created: ~/Documents/blog/source/categories/index.md
```

找到這個 index.md 後會看到預設內容，在下面新增一行 type: "categories"

```yml
---
title: categories
date: 2018-05-10 11:27:42
type: "categories"
---
```

tags 頁面比照 categories 作法：
```
$ hexo new page tags
```
```yml
---
title: tags
date: 2018-05-10 11:27:42
type: "tags"
---
```

要將文章歸類或附上標籤的話，只要在 post head 上新增 categories 跟 tags，格式如下
```md
---
title: 談談 Hexo 怎麼配置
date: 2018-05-10 14:52:29
tags: 
- markdown
categories: 
- Note
---
```
**需要注意的是 tags 可以一直往下新增，但 categories 只會讀第一行去分類！**

*搜尋功能我還在摸索 ... 只是開著看起來比較完整但千萬不要去按*

<small>[來源參照](https://linlif.github.io/2017/05/27/Hexo%E4%BD%BF%E7%94%A8%E6%94%BB%E7%95%A5-%E6%B7%BB%E5%8A%A0%E5%88%86%E7%B1%BB%E5%8F%8A%E6%A0%87%E7%AD%BE/)</small>


## 隱藏文章

Hexo 原先有一個新增 draft 文件的功能，但常常有些舊文章或已經發布的文章就是突然想要偷偷藏起來，就非常需要這個功能，先打開 themes/next/index.swig

```swig
{% extends '_layout.swig' %}
{% import '_macro/post.swig' as post_template %}
{% import '_macro/sidebar.swig' as sidebar_template %}

{% block title %}{{ config.title }}{% if theme.index_with_subtitle and config.subtitle %} - {{config.subtitle }}{% endif %}{% endblock %}

{% block page_class %}
  {% if is_home() %}page-home{% endif -%}
{% endblock %}

{% block content %}
  <section id="posts" class="posts-expand">
    {% for post in page.posts %}
      {{ post_template.render(post, true) }}
    {% endfor %}
  </section>

  {% include '_partials/pagination.swig' %}
{% endblock %}

{% block sidebar %}
  {{ sidebar_template.render(false) }}
{% endblock %}
```

這份文件就是負責顯示你的文章列表，可以看到 'for post in page.posts' 這個 for loop 把每一個文章都吐出來，所以要在這邊下個判斷是來篩選掉要隱藏的文章 (我用 visible: boolean 變數名稱可自訂)
```swig
{% extends '_layout.swig' %}
{% import '_macro/post.swig' as post_template %}
{% import '_macro/sidebar.swig' as sidebar_template %}

{% block title %}{{ config.title }}{% if theme.index_with_subtitle and config.subtitle %} - {{config.subtitle }}{% endif %}{% endblock %}

{% block page_class %}
  {% if is_home() %}page-home{% endif -%}
{% endblock %}

{% block content %}
  <section id="posts" class="posts-expand">
    {% for post in page.posts %}
      {% if (post.visible) %}
        {{ post_template.render(post, true) }}
      {% endif %}
    {% endfor %}
  </section>

  {% include '_partials/pagination.swig' %}
{% endblock %}

{% block sidebar %}
  {{ sidebar_template.render(false) }}
{% endblock %}
```

接著到文章內來定義 visible

```md
---
title: 談談 Hexo 怎麼配置
date: 2018-05-10 14:52:29
visible: true
tags: 
- markdown
categories: 
- Note
---
```

到這邊列表頁就會去篩選每一篇文章的 visible 來決定顯示與否，但這樣還有一個麻煩處：
**我每次 hexo new 一個新的 post 都要去加他**

於是更懶也更進階的方式就是在每個 post 生成的 template 加入這個變數，打開 /scaffolds/post.md 看到如下的預設值
```md
---
title: {{ title }}
date: {{ date }}
---
```
這就是每次 hexo new post-name 生出來的 post-name.md 中的架構，只要對他動點手腳
```md
---
title: {{ title }}
date: {{ date }}
tags:
categories: 
visible: false
---
```
我順便把 tags 跟 categories 的定義一併加上去，這樣就不需要每次都手動去設定 true false 了！

<small>[來源參照](http://forwardkth.github.io/2016/05/08/next-theme-post-visibility/)</small>

## 代碼背景顏色設定

這部分算是選配，next 預設代碼背色是白的，但個人比較偏好深色系，所以只好動手了 ...
到  <font style='color: white; background-color: #156c08; padding: 0 5px; border-radius: 3px;'>Next config</font> 找出下面這段就很清楚了 (註解都有寫)
```yml
# Code Highlight theme
# Available values: normal | night | night eighties | night blue | night bright
# https://github.com/chriskempson/tomorrow-theme
highlight_theme: night
```