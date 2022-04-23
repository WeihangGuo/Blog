+++ 
draft = true
date = 2022-04-24T04:17:23+08:00
title = ""
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++

# Hugo对Bilibili视频嵌入的支持

有时候写博客的时候想插入一些bilibili的视频作为成品展示或者demo。但是hugo原生只支持youtube并不支持bilibili。每次写HTML链接视频不但麻烦还不优雅，偶然我看到hugo里有一个shortcode的功能。Shortcodes类似于一种标记，Hugo会在生成网站时使用对应的HTML模板替换该位置的Shortcodes代码，然后再生成真正的网站。用这个功能就可以一行代码嵌入bilibili的视频了。成果如下：

{{<bilibili BV1Yt411d7xn>}}

实现这个效果只需要 \{\{\<bilibili BV1Yt411d7xn\>\}\}

---

实现： 

在``/layouts/shortcodes``路径下创建一个`bilibili.html`，读者也可以用别的名字，但是这个文件的名字一定是要和\{\{\<bilibili BV1Yt411d7xn\>\}\}的第一个参数相同。 

之后在`/layouts/shortcodes/<SHORTCODE>.html`中添加如下代码：

```html
<style>
    .aspect-ratio {
        position: relative;
        width: 100%;
        height: 0;
        padding-bottom: 75%;
        }
          
    .aspect-ratio iframe {
        position: absolute;
        width: 100%;
        height: 100%;
        left: 0;
        top: 0;
        }
    </style>
          
    
    <div align=center class="aspect-ratio">
        <iframe src="https://player.bilibili.com/player.html?bvid={{ index .Params 0 }}&&page=1&as_wide=1&high_quality=1&danmaku=0" 
        scrolling="no" 
        border="0" 
        frameborder="no" 
        framespacing="0" 
        allowfullscreen="true"> 
        </iframe>
    </div>
```

---

如果你的主题开启了`csp`，那么你还需要在`framesrc`中添加`"https://player.bilibili.com/"`否则视频会被屏蔽。

---

在更改了`bilibili.html`之后，你就可以在博客中输入\{\{\<bilibili BV号\>\}\}来插入视频了。当然，如果你想用`cid`或者`aid`来插入视频可以把`line 20`中的`bvid`改成`cid`或者`aid`。 