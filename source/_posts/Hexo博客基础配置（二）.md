---
title: Hexo博客基础配置（二）
date: 2021-02-27 16:00:00
categories:
    - 个人博客搭建
tags:
    - Hexo
---
## 基础设置

### Site 站点配置文件

```yml
# Site
title: Mw-Blog
subtitle:
description: 个人博客
keywords:
author: Mw
language: zh-CN
timezone:
```

### 侧边栏 <mark>主题配置文件</mark>

```yml
sidebar:
# Sidebar Position.
position: right  //右侧
#position: right
```

### 头像

```yml
avatar:
# In theme directory (source/images): /images/avatar.gif
# In site directory (source/uploads): /uploads/avatar.gif
# You can also use other linking images.
url: /images/Mw.png
# If true, the avatar would be dispalyed in circle.
rounded: true
# If true, the avatar would be rotated with the cursor.
rotated: false
```

### 浏览进度百分比

<mark>主题配置文件</mark>

```yml
# Scroll percent label in b2t button.
scrollpercent: true
```

## 个性化设置（进阶）

### 统计访客人数

```yml
busuanzi_count:
enable: true
total_visitors: true
total_visitors_icon: fa fa-user
total_views: true
total_views_icon: fa fa-eye
post_views: false
post_views_icon: fa fa-eye
```

### 顶部背景 菜单栏阴影

My-Blog\themes\hexo-theme-next\source\css\_common\outline\header\header.styl中

```styl
.header {
background: url("../images/bg.jpg") no-repeat 50% 0;
height: 600px;
}
```

```styl
.menu {
margin-top: 20px;
padding-left: 0;
text-align: center;
background: rgba(255,255,255,0.65);
margin-left: auto;
margin-right: auto;
width: 300px;
}
```

### **添加统计文章字数和文章阅读时间**

1. 安装插件

    ```cmd
    npm install hexo-wordcount --save
    npm install hexo-symbols-count-time --save
    npm install eslint --save
    ```

2. <mark>站点配置文件</mark>添加

    ```yml
    symbols_count_time:
    symbols: true                # 文章字数统计
    time: true                   # 文章阅读时长
    total_symbols: true          # 站点总字数统计
    total_time: true             # 站点总阅读时长
    exclude_codeblock: false     # 排除代码字数统计
    ```

3. <mark>站点配置文件</mark>添加

    ```yml
    symbols_count_time:
        separated_meta: true     # 是否另起一行（true的话不和发表时间等同一行）
        item_text_post: true     # 首页文章统计数量前是否显示文字描述（本文字数、阅读时长）
        item_text_total: true   # 页面底部统计数量前是否显示文字描述（站点总字数、站点阅读时长）
        awl: 4                  # Average Word Length
        wpm: 275                 # Words Per Minute（每分钟阅读词数）
        suffix: mins.

    post_wordcount:    # 字数统计
        item_text: true    # 是否显示文字
        wordcount: true    # 显示字数
        min2read: true     # 显示阅读时间
        totalcount: true    # 显示总数
        separated_meta: true   # 是否分开
    ```

### 修改底部顺序 由于卜算子是单独的一个文件 文件读取靠前

找到busuanzi-counter.swig文件  把文件内容拷贝到footer.swig文件中  调整模块div位置即可

删除themes\hexo-theme-next\layout\_third-party\statistics\index.swig中对busuanzi-counter.swig文件的引用

```html
    {%- if theme.footer.beian.enable %}
    <div class="beian">
        {{- next_url('https://beian.miit.gov.cn', theme.footer.beian.icp + ' ') }}
        {%- if theme.footer.beian.gongan_icon_url %}
        <img src="{{ url_for(theme.footer.beian.gongan_icon_url) }}" style="display: inline-block;">
        {%- endif %}
        {%- if theme.footer.beian.gongan_id and theme.footer.beian.gongan_num %}
        {{- next_url('http://www.beian.gov.cn/portal/registerSystemInfo?recordcode=' + theme.footer.beian.gongan_id, theme.footer.beian.gongan_num + ' ') }}
        {%- endif %}
    </div>
    {%- endif %}

    <div class="copyright">
    {% set copyright_year = date(null, 'YYYY') %}
    &copy; {% if theme.footer.since and theme.footer.since != copyright_year %}{{ theme.footer.since }} – {% endif %}
    <span itemprop="copyrightYear">{{ copyright_year }}</span>
    <span class="with-love">
        <i class="{{ theme.footer.icon.name }}"></i>
    </span>
    <span class="author" itemprop="copyrightHolder">{{ theme.footer.copyright or author }}</span>
    </div>


    <div class="wordcount">
    {%- if config.symbols_count_time.total_symbols %}
        <span class="post-meta-item-icon">
        <i class="fa fa-chart-area"></i>
        </span>
        {%- if theme.symbols_count_time.item_text_total %}
        <span class="post-meta-item-text">{{ __('symbols_count_time.count_total') + __('symbol.colon') }}</span>
        {%- endif %}
        <span title="{{ __('symbols_count_time.count_total') }}">{{ symbolsCountTotal(site) }}</span>
    {%- endif %}

    {%- if config.symbols_count_time.total_time %}
        <span class="post-meta-divider">|</span>
        <span class="post-meta-item-icon">
        <i class="fa fa-coffee"></i>
        </span>
        {%- if theme.symbols_count_time.item_text_total %}
        <span class="post-meta-item-text">{{ __('symbols_count_time.time_total') }} &asymp;</span>
        {%- endif %}
        <span title="{{ __('symbols_count_time.time_total') }}">{{ symbolsTimeTotal(site, config.symbols_count_time.awl, config.symbols_count_time.wpm, __('symbols_count_time.time_minutes')) }}</span>
    {%- endif %}
    </div>


    {%- if theme.busuanzi_count.enable %}
    <div class="busuanzi-count">
    <script{{ pjax }} async src="https://busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>

    {%- if theme.busuanzi_count.total_visitors %}
        <span class="post-meta-item" id="busuanzi_container_site_uv" style="display: none;">
        <span class="post-meta-item-icon">
            <i class="{{ theme.busuanzi_count.total_visitors_icon }}"></i>
        </span>
        <span class="site-uv" title="{{ __('footer.total_visitors') }}">
            <span id="busuanzi_value_site_uv"></span>
        </span>
        </span>
    {%- endif %}

    {%- if theme.busuanzi_count.total_visitors and theme.busuanzi_count.total_views %}
        <span class="post-meta-divider">|</span>
    {%- endif %}

    {%- if theme.busuanzi_count.total_views %}
        <span class="post-meta-item" id="busuanzi_container_site_pv" style="display: none;">
        <span class="post-meta-item-icon">
            <i class="{{ theme.busuanzi_count.total_views_icon }}"></i>
        </span>
        <span class="site-pv" title="{{ __('footer.total_views') }}">
            <span id="busuanzi_value_site_pv"></span>
        </span>
        </span>
    {%- endif %}
    </div>
    {%- endif %}


    {%- if theme.footer.powered %}
    <div class="powered-by">
        {%- set next_site = 'https://theme-next.org' %}
        {%- if theme.scheme !== 'Gemini' %}
        {%- set next_site = 'https://' + theme.scheme | lower + '.theme-next.org' %}
        {%- endif %}
        {{- __('footer.powered', next_url('https://hexo.io', 'Hexo', {class: 'theme-link'}) + ' & ' + next_url(next_site, 'NexT.' + theme.scheme, {class: 'theme-link'})) }}
    </div>
    {%- endif %}

    {%- if theme.add_this_id %}
    <div class="addthis_inline_share_toolbox">
        <script src="//s7.addthis.com/js/300/addthis_widget.js#pubid={{ theme.add_this_id }}" async="async"></script>
    </div>
    {%- endif %}


    {{- next_inject('footer') }}

```

### 设置阅读全文

打开<mark>主题配置文件</mark>，修改auto_excerpt:字段为true，length表示显示文本的长度

在想要隐藏的位置加入以下代码：

```md
<!--more-->
```

### 添加文章评论和阅读次数

1. 创建LeanCloud账号
2. 创建应用-创建存储-结构化数据-创建Class

### 文章插入图片，点击查看大图

#### 文章插入图片

1. 绝对路径本地引用(不采用)

    ```html
    ![](/images/image.jpg)
    ```

    不推荐 图片存放过多后不便于查找

2. 相对路径引用(不采用)

    开启<mark>站点配置文件</mark>中的post_asset_folder:true,执行命令$ hexo new post_name，在source/_posts中会生成文章post_name.md和同名文件夹post_name。将图片资源放在post_name中，文章就可以使用相对路径引用图片资源了。

    ```html
    ![](image.jpg)
    ```

    这种相对路径的图片显示方法在博文详情页面显示没有问题，但是在首页预览页面图片将显示不出来。

3. 标签插件语法引用(采用)

    标签插件语法，可以使图片在文章和首页中同时显示。

    ```html
    # 本地图片资源，不限制图片尺寸
    {% asset_img image.jpg This is an image %}
    # 网络图片资源，限制图片显示尺寸
    {% img http://www.viemu.com/vi-vim-cheat-sheet.gif 200 400 vi-vim-cheat-sheet %}

    ```

#### 点击查看大图

开启<mark>主题配置文件</mark>中的fancybox: true即可

fancybox:[https://github.com/theme-next/theme-next-fancybox3](https://github.com/theme-next/theme-next-fancybox3)

参考：[https://www.jianshu.com/p/c79fd3f7fdfa#fn1](https://www.jianshu.com/p/c79fd3f7fdfa#fn1)
