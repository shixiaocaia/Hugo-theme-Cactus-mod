---
title: Hexo-AirCloud主题装修指南
tags:
  - 博客
slug: 53903
date: 2022-11-17 12:09:15
---

> 考完了矩阵论，正逢阴雨天，不去工位了，把这篇装修写了，希望能帮助到使用这个主题的大家，不知道为什么我添加谷歌收录没有成功，可能也没什么人能检索到这篇文章。

## 引入Waline

参考[waline官方文档](https://waline.js.org/)，在layout.ejs中引入以下代码：

```shell
<%if( (is_post()  || is_current('about')) && config.comment && config.comment.type === 'waline'){%>
    <head>
        <!-- ... -->
        <link
          rel="stylesheet"
          href="https://unpkg.com/@waline/client@2.13.0/dist/waline.css"
        />
        <!-- ... -->
      </head>
      <body>
        <!-- ... -->
        <div id="waline"></div>
        <script type="module">
          import { init } from 'https://unpkg.com/@waline/client@2.13.0/dist/waline.mjs';
      
          init({
            el: '#waline',
            serverURL: 'yourownurl',
            emoji: [
                '/img/blobs-png/', # emoji文件看你个人的
            ],
          });
        </script>
      </body>
      
<% } %>
```

- 个人添加了emoji blobs-png选项。
- serverURL 绑定个人的地址，注意vercel域名被污染了，要绑定自己的域名。
- 这里将client的版本限定为2.13.0，是因为2.13.1版本存在相应的bug。
- 再去`post.ejs`和`about.ejs`中添加。

```js
<div id="waline_thread"></div>
<div id="waline"></div>
```

![TinySnap-2022-11-17-13.39.24](https://bu.dusays.com/2022/12/24/63a6b5e2a725d.png)

## 插件

```shell
npm i hexo-slug
npm i hexo-generator-feed
npm i hexo-deployer-upyun-purge
npm i hexo-wordcount
```


### RSS

1. 安装npm包`npm i hexo-generator-feed。`

2. 在config中添加

```shell
feed:
  type: atom
  path: atom.xml
  limit: false
  content: true
  icon: https://fastly.jsdelivr.net/gh/cdn-x/xaoxuu/avatar/rect-256@2x.png
  autodiscovery: true
```

3. 在合适的位置添加RSS标志，🔗为你的域名+atom.xml，例如`https://shixiaocaia.fun/atom.xml`。

### permalink

安装npm包`npm i hexo-slug`。

修改config文件中`permalink: posts/:slug/`。

有利于简化网页链接信息，例如，`https://shixiaocaia.fun/posts/36833/`，每次deploy时候，会自动生成一个slug。

### deployer - upyun

```shell
deploy:
  - type: upyun
    serviceName: blogcjm-upyun
    operatorName: shixiaocaia
    operatorPassword: XXXXXXXXXXXXXXXX
    path: /
    cacheUrl: https://shixiaocaia.fun  
```

白嫖upyun联盟的云存储、CDN啥的不香吗！而且之前有问题，询问他们，态度超级好！（尽管我是白嫖用户

甚至上次我问能不能去掉upyun的页脚的图标（upyun联盟的要求页脚加入链接），因为对不齐很不美观，客服也爽快的说可以！

### wordcount

见[源文件](https://github.com/willin/hexo-wordcount)。

```shell
<span class="post-count"><%= totalcount(site) %></span>
```

## 页脚

- iconfont等图标，可以通过[阿里巴巴矢量图库](https://www.iconfont.cn/)获取，非商用可以免费使用。

  - 在图库中下单中后，再到`head.ejs`中添加你自己的`  <link href="//at.alicdn.com/t/c/font_3734178_jokajfqlisd.css" rel="stylesheet" type="text/css">`即可。

  - 然后在`footer.ejs`中模仿现有的图标，改成自己想要的iconfont，比如我加了RSS的，Docsify的。

- 站点的信息可以直接抄用，之前也说过了，这里就不写了。

- 访客信息，使用的是[杜老师自建国内不蒜子统计平台](https://dusays.com/476/)，快去冲！

- 字数统计，使用hexo-wordcount。

我使用的页脚信息

```shell
<span id="copyright"></span> <span id="days"></span> , 嘟嘟了<span class="post-count"><%= totalcount(site) %></span>字 , 有了<span id="busuanzi_value_site_pv"></span>次驻足。

<script>
    function createtime() {
        var s1 = '2022-06-03';//设置为建站时间
        s1 = new Date(s1.replace(/-/g, "/"));
        s2 = new Date();
        var year = s2.getFullYear();
        var days = s2.getTime() - s1.getTime()+250;
        var number_of_days = parseInt(days / (1000 * 60 * 60 * 24));
        document.getElementById("days").innerHTML = "在宇宙中漂流了 " + number_of_days + " 天 ";
        if(year == 2022){
           document.getElementById("copyright").innerHTML = "© " + "2022";
        }
        else{
           document.getElementById("copyright").innerHTML = "© " + "2022 - " + year;
        }
        
    };
setInterval("createtime()",250);
</script>
<script async src="https://npm.elemecdn.com/penndu@1.0.0/bsz.js"></script>
```

![TinySnap-2022-11-19-20.21.10](https://bu.dusays.com/2022/12/24/63a6b5e6eb871.png)

## 图片居中

原来的图片显示是偏左的，当你的图片过小时，显得很奇怪。

修改`aircloud.css`中的

```shell
.post-content p img {
  max-width: 80%;
  cursor: pointer;
  display: block;
  margin: 0 auto;  //修改
}
```

> 试着F12控制台找相关部分，再去VS检索相应的部分，是很有用的方法！

## 结尾

> 当我刚换了aircloud后，stellar引来一大波更新，社区十分活跃，很羡慕了。
>
> 没准不久就换回去了。
