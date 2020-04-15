
# HTML基础

## HTML语义化的理解?

语义化是指使用恰当语义的html标签，让页面具有良好的结构与含义，比如<p>标签就代表段落，<article>代表正文内容等等。富文本类的应用很重要,利于SEO
语义化的好处主要有两点：
1. 开发者友好：使用语义类标签增强了可读性，开发者也能够清晰地看出网页的结构，也更为便于团队的开发和维护
2. 机器友好：带有语义的文字表现力丰富，更适合搜索引擎的爬虫爬取有效信息，语义类还可以支持读屏软件，根据文章可以自动生成目录


## src和href的区别？

1. src是指向外部资源的位置，指向的内容会嵌入到文档中当前标签所在的位置，在请求src资源时会将其指向的资源下载并应用到文档内，
如js脚本，img图片和frame等元素。【当浏览器解析到该元素时，会暂停其他资源的下载和处理，直到将该资源加载、编译、执行完毕】，所以一般js脚本会放在底部而不是头部。

2. href是指向网络资源所在位置（的超链接），用来建立和当前元素或文档之间的连接，当浏览器识别到它他指向的文件时，就会并行下载资源，不会停止对当前文档的处理。

## img的srcset的作用是什么？

可以设计响应式图片，我们可以使用两个新的属性srcset 和 sizes来提供更多额外的资源图像和提示，帮助浏览器选择正确的一个资源。
属性格式：图片地址 宽度描述w 像素密度描述x，多个资源之间用逗号分隔
srcset 定义了我们允许浏览器选择的图像集，以及每个图像的大小。
sizes 作用就在于告诉浏览器根据屏幕尺寸和像素密度的计算值从srcset 中选择最佳的图片源。
`
<img src="clock-demo-thumb-200.png"
     alt="Clock"
     srcset="clock-demo-thumb-200.png 200w,
             clock-demo-thumb-400.png 400w"
     sizes="(min-width: 600px) 200px, 50vw">
     < img src="density-x1.jpg" 
          srcset="density-x1.jpg 1x,
                  density-x2.jpg 2x, 
                  density-x3.jpg 3x" />
`
有了这些属性，浏览器会：

查看设备宽度
检查 sizes 列表中哪个媒体条件是第一个为真
查看给予该媒体查询的槽大小
加载 srcset 列表中引用的最接近所选的槽大小的图像

## 还有哪一个标签能起到跟srcset相似作用？

<picture>元素通过包含零或多个 <source> 元素和一个 <img>元素来为不同的显示/设备场景提供图像版本。

`<picture>
    <source srcset="/media/examples/surfer-240-200.jpg"
            media="(min-width: 800px)">
    <img src="/media/examples/painted-hand-298-332.jpg" />
</picture>`


## script标签中defer和async的区别 ？

defer：浏览器指示脚本在文档被解析后执行，script被异步加载后并不会立刻执行，而是等待文档被解析完毕后执行。
async：同样是异步加载脚本，区别是【脚本加载完毕后立即执行】，这导致async属性下的脚本是乱序的，对于script有先后依赖关系的情况，并不适用。


## 有几种前端储存的方式？
cookies、localStorage、sessionStorage、Web SQL、IndexedDB


##这些方式的区别是什么？（追问）

cookies： 在HTML5标准前本地储存的主要方式，优点是兼容性好，请求头自带cookie方便，缺点是大小只有4k，自动请求头加入cookie浪费流量，每个domain限制20个cookie，使用起来麻烦需要自行封装

localStorage：HTML5加入的以键值对(Key-Value)为标准的方式，优点是操作方便，永久性储存（除非手动删除），大小为5M，兼容IE8+

sessionStorage：与localStorage基本类似，区别是sessionStorage当页面关闭后会被清理，而且与cookie、localStorage不同，他不能在所有同源窗口中共享，是会话级别的储存方式

IndexedDB： 是被正式纳入HTML5标准的数据库储存方案，它是NoSQL数据库，用键值对进行储存，可以进行快速读取操作，非常适合web场景，同时用JavaScript进行操作会非常方便。














































