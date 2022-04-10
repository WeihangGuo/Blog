+++ 
draft = false
date = 2022-04-10T12:06:26+08:00
title = "Change Hugo version when deploying with Vercel"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++

# Change Hugo version when deploying with Vercel

The default hugo version is 0.58.2, which is too old to support some new feature and themes. The [instruction](https://vercel.com/guides/deploying-hugo-with-vercel) on the vercel's website is very simple and does not include how to change the hugo version. The method online doesn't seem to work neither (Maybe it works, but it is too complex). When I want to give up and use another platform to deploy my blog, I found [cloudflare pages](https://pages.cloudflare.com/)' documentation is clear (see [here](https://developers.cloudflare.com/pages/framework-guides/deploy-a-hugo-site/)) and include how to change the hugo version. I tried the same environment variable in vercel, and it works. 

## Add an environment variable

![image-20220410115114570](https://raw.githubusercontent.com/baboonSTW/Blog-img/main/image-20220410115114570.png)

 

## Another possible method (Has some errors)

**Install npm**

- Download node.js from: https://nodejs.org/en/download/
- Verify Installation

![image-20220409122103135](https://raw.githubusercontent.com/baboonSTW/Blog-img/main/image-20220409122103135.png)

**Install Vercel CLI**

- Instruction can be found at: https://vercel.com/docs/cli. I summarize some key steps. 

- ```npm i -g vercel```

![image-20220409122621668](https://raw.githubusercontent.com/baboonSTW/Blog-img/main/image-20220409122621668.png)

- Relate local folder with a vercel project `vercel`

- Create a `vercel.json` in a proper location and add the following content to the file:

- ```
  {
      "build": {
          "env": {
              "HUGO_VERSION": "0.96.0"
          }
      }
  }
  ```

- Run `vercel --local-config /path-to/vercel.json`

With the above steps, you **should** be able to successfully deploy your website. However I get the follow error:

![image-20220410003814222](https://raw.githubusercontent.com/baboonSTW/Blog-img/main/image-20220410003814222.png)

It seems like hugo need `.git` to work. However, I cannot (or it is inappropriate) to upload `.git` folder. If you know how to solve this error, please help! Thanks. 

**References:**

https://discourse.gohugo.io/t/vercel-tips/34766

https://github.com/vercel/vercel/discussions/5834

