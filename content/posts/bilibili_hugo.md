+++ 
draft = false
date = 2022-04-24T04:17:23+08:00
title = "Hugo's Support for Bilibili Video Embedding"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++

# Hugo's Support for Bilibili Video Embedding

Sometimes when writing a blog, you might want to insert some Bilibili videos as product demonstrations or demos. However, Hugo natively only supports YouTube, not Bilibili. Writing HTML links for the videos each time is not only troublesome but also inelegant. By chance, I discovered that Hugo has a feature called shortcodes. Shortcodes act as a kind of marker, with Hugo using the corresponding HTML template to replace the shortcodes in the site generation process, thus generating the real site. This feature allows you to embed Bilibili videos with just one line of code. The result is as follows:

{{<bilibili BV1Yt411d7xn>}}

To achieve this, you only need \{\{\<bilibili BV1Yt411d7xn\>\}\}

---

Implementation:

Create a `bilibili.html` under the ``/layouts/shortcodes`` path. Readers can use other names, but the name of this file must match the first argument of \{\{\<bilibili BV1Yt411d7xn\>\}\}.

Then add the following code to `/layouts/shortcodes/<SHORTCODE>.html`:

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

If your theme has `csp` enabled, then you also need to add `"https://player.bilibili.com/"` to `framesrc`, otherwise the video will be blocked.

---

After changing `bilibili.html`, you can enter \{\{\<bilibili BV number\>\}\} in your blog to embed a video. Of course, if you want to use `cid` or `aid` to embed videos, you can change `bvid` on `line 20` to `cid` or `aid`.