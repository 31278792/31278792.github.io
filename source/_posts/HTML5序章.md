---
title: HTML5序章
date: 2020-02-18 10:02:16
tags:
	- HTML
	- 标记语言
categories: 前端
password: 31278792
---
**一天天宅着蛮枯燥的，HTML5地学习或许能打破这乏味的时光。以前,刚开始学习 HTML ，才明白前端界面是如何实现的，当然也少不了CSS、JavaScript。像我们这博客完全是通过这3样语言实现的静态网页，瞬间感觉前端是不是比后端的东西，更直观地看得出成果。你光使用后端语言是无法完成Web网页开发，当然，如果有后端语言与数据库实现的自然是动态网页。话不多说，言归正传，开始我们今天HTML5的学习！**
## HTML5概述
### HTML(HyperText Markup Language)
  **HTML全称翻译而来是一款超文本标记语言，经过多过版本的更新，最后到现在HTML5，其实目的就是升级优化添加新功能，目前主要优势表现在终端上，跨平台、跨分辨率、版本控制简单**
  **HTML5 实际上指的是包括HTML、CSS和JS在内的一套技术组合，能够减少浏览器对于需要插件(这也是显著功能)的丰富性网络应用服务**

<!--more-->

### 标记语言与编程语言区别
  ***觉得这个有必要记录下，标记语言强调属性，没有逻辑结构；而编程语言有逻辑和行为能力***
  
### HTML5的八大特性
**1)语义特性**
**2)离线存储特性**
**3)设备访问特性**
**4)通讯特性**
**5)多媒体特性**
**6)三维及图形特性**
**7)性能及基础特性**
**8)CSS3特性**

### HTML5的优点
#### 标签丰富、功能强大
通过HTML5可以轻松实现页面中的音频、视频、动画等效果。
#### 支持移动互联网的Web应用需求
HTML5开发应用，可以轻松运行于手机、平板电脑等移动设备。
#### 适应当前Web应用发展潮流
HTML5应用界面友好，功能强大，是今后Web发展的主流方向。
#### 解决了跨浏览器问题
HTML5之前各大浏览器的竞争，在各自浏览器增加各种各样的功能，且不具有统一的标准。用不同浏览器看到不同页面效果。在HTML5中，纳入了所有合理的扩展功能，具备良好的跨平台性能。针对不支持新标签的老式IE浏览器，只需简单地添加JavaScript代码就可以使用新的元素
#### 新增了多个新特性
##### 良好的语义特性
HTML5支持数据与微格式，增加的新元素赋予网页更好的意义和结构。增加了很多新的结构元素，从而使文档结构更加清晰明确，新增的结构元素包括section元素、article元素、nav元素以及aside元素等
##### 强大的绘图功能
既可以通过Canvas API动态地绘制各种效果精美的图形，也可以通过SVG绘制各种伸缩矢量图形
##### 增强的音频视频播放和控制功能
HTML5新增了audio和video元素，可以不依赖任何插件而播放音频和视频
##### HTML5的数据存储和数据处理的功能
(1)离线应用 (2)Web通信 (3)本地存储
##### 获取地理位置信息
HTML5新增了Geolocation API规范，可以通过浏览器获取用户的地理位置信息
##### 提高页面相应的多线程
HTML5新增Web Workers来实现多线程功能。通过Web Workers将耗时长的处理交给后台线程，降低Web服务的响应时间，有利于增强用户体验
##### 文件 API
FileReader API：简化用户访问本地/服务器端文件系统的相关处理
FileSystem API：给文件系统添加浏览器保护，使得文件系统数据可以永久本地保存
#### 用户优先的原则
##### 安全机制的设计
##### 表现和内容分离
#### 化繁为简的优势
新的简化的字符集声明、新的简化的DOCTYPE、简单而强大的HTML5 API及以浏览器原生能力替换复杂的JavaScript代码。为了避免造成误解，HTML5对每一个细节都有着非常明确的规范说明，不允许有任何的歧义和模糊出现

### HTML5支持情况
  就目前Chrome52(谷歌)与FireFox48(火狐)两个浏览器支持项数最多，当然这毕竟程序员受欢迎程度高的两个浏览器。
#### 检测指定元素的DOM对象是否被浏览器支持
如：canvas、video、audio等
#### 检测浏览器是否支持指定元素的对象的方法
如：canvas.getContext (canvas这个元素对图像操作功能是真的强大，就这博客动态背景粒子效果也是通过它实现的)
#### 检测全局对象是否拥有特定属性
如：navigator.geolocation()
#### 使用HTML特性检测工具(Modernizr)
##### 检测浏览器对HTML5元素的支持情况
[Can I use](http://caniuse.com)
##### 检测浏览器对HTML5的支持情况
[HTML5TEST](http://html5test.com)
##### 检查HTML5内容是否完全符合规范
[The W3C Markup Validation Service](http://validator.w3.org)

### 开发工具
开发工具这就很多了，对于前端学习小白当然是用Dreamweaver甚好，当然WebStorm、VSCode、VS2019(微软的东西，值得一试)等开发工具也可以尝试

### 帮助文档
[W3C](http://www.w3school.com.cn/)
[Mozilla](http://developer.mozilla.org)

## HTML5的变化
### 标记的改进
#### DOCTYPE声明
`<!DOCTYPE html>`
#### 指定字符编码
`<meta charset=utf-8>`
#### 标签不需要区分大小写
#### 可以省略的标签
##### 不允许写结束标签
area、base、br、col、hr、img、input、meta、link、param、embed、keygen、track、source等
##### 可以省略的结束标签
dt、dd、li、p、rt、rp、option、optiongroup、colgroup、thread、tbody、tfoot、tr、td、th等
##### 开始标签和结束标签均可省略
html、head、body、tbody、thead、colgroup等
#### Boolean属性设置
checked、selected、multiple、readonly、和disable等
##### 当只写属性而不指定属性值时，表示属性值为true
##### 如果想要将属性值设为false，可以不使用该属性
##### 若属性值为true时，也可以将属性名设定为属性值，或空字符串
#### 属性引号
  当属性不包含一些特殊字符(空字符串、<、>、=、单引号、双引号)，引号可以省略。示例：<meta charset="utf-8">
#### 废弃的元素
##### 使用CSS替换的元素
basefont、big、center、font、strike、s、tt、u等
##### 停用frame框架
frame主要对页面进行分割，但由于frame不便于搜索引擎的抓取和优化，故不再支持
##### 只有部分浏览器支持的元素
bgsound、marquee、applet等
#### 新增的元素（*功能都很强大*）
##### 结构元素
section、article、aside等
##### 多媒体元素
video、audio、embed等
##### 表单标签
input
##### 扩展HTML功能的元素
canvas等
#### 全局属性
1)contenteditable属性
2)contextmenu属性
3)data-*属性
4)spellcheck属性
5)draggable属性
6)hidden属性

## HTML5文档结构
**HTML文档中元素：从开始标记到结束标记的所有代码**
**元素的内容：开始标记与结束标记之间的内容**
**元素属性：用来说明元素的特征，每个属性总是对应一个属性值，称为"属性/值"**
### head标签
文档头部信息，标签内容包含字符编码设置、标题、关键字与网页描述等
### body标签
网页主体信息（我们上网所见所听所感信息都来源于body里）
其中`<header></header>、<main></main>、<footer></footer>`等新增标签语义清晰，利于SEO，利于盲人阅读，无法用这些新增标签表达的结构还是用<div>
#### `<header></header>`标签
可以是整个页面或者页面区域的一个头部信息（标题）
可以包含：logo、标题、搜索、导航、通知、帐号等
至少包含一个标题(h1~h6)，还可以包含hgroup、nav等
*注意：跟 `<head></head>` 标签的区别*
#### `<main></main>`标签
页面中的主体部分。不能包含公共导航、logo、版权信息等公共内容
*注意：一个页面只能有一个main标签。`<main>`元素不能是以下元素的后代：`<article>、<aside>、<footer>、<header>或<nav>`*
#### `<footer></footer>`标签
页面或页面某个板块的底部部分
通常包含文档的作者、版权信息、使用条款链接、联系信息等等
*可以在一个文档中使用多个 `<footer>`元素*
#### `<nav></nav>`标签
导航(navigation)的简写
专门用来包裹导航的内容。包括顶部导航、底部导航、侧边栏导航、翻页导航等
#### `<article></article>`标签
文档、页面或者是应用程序当中的独立完整可以被外部引用的内容
它可以是一篇文章、一篇帖子、一段评论或者是独立的插件形式出现
*`<article><article>`里面也可以使用 `<header>、<footer>以及<article>`标签*
#### `<section></section>`标签
定义了文档的某个区域。比如章节、头部、底部或者文档的其他区域（即整体中的某部分）
*`<section></section>`里也可以有 `<article></article>`标签*
#### `<aside></aside>`标签
定义`<article>`标签外的内容。广告，侧边栏等
#### `<hgroup></hgroup>`标题组
对网页或区段(section)的标题进行组合。一个区域中包括了主标题和至少一个子标题的时候才用
*注意：单独一个标题的时候，不用 `<hgroup></hgroup>`*
#### `<time></time>`时间
主要是使页面语义描述更清晰，便于网络搜索引擎更好的理解网页页面
time可以为内容添加如发布时间，事件发生时间等信息。
拥有datetime和pubdate两个属性
#### `<address></address>`联系信息
示例：
```
<address>
  <ul>
    <li>网站：http://www.w3school.com.cn/</li>
    <li>GitHub:XXXXXXXX</li>
	<li>E-mail:xxxxxxx@qq.com</li>
  </ul>
</address>
```

#### 其他元素
##### bdi:双向隔离，将指定文本脱离父元素的文本方向
##### details和summary：描述文档某个部分的细节。summary作为detail的标题部分。只有一个open属性
##### figure和figcapton：用于规定独立的流内容（图像、图片、照片、代码等），figcaption定义标题部分
##### ruby、rp和rt：用于定义注释，如中文注音。它由解释文本和解释内容两部分构成。Rt未来提供解析内容
##### progress和meter：progress用于表示任务的进度，而meter用来度量已知范围或分数值内的标量
##### dialog：定义一个对话框，只有open属性
##### mark和wbr：mark用于定义带有标记的文本。wbr用户规定在文本的何处适合添加换行符，br则是必须换行