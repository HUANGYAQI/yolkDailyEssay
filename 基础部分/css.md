
# CSS基础

## 选择器的优先级是怎样的？
 !important > 内联 > ID选择器 > 类选择器 > 标签选择器
 
## link与@import
link属于XHTML标签，而@import是CSS提供的。
页面被加载时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载。

## 隐藏页面元素
1. opacity:0：本质上是将元素的透明度将为0，就看起来隐藏了，但是依然占据空间且可以交互
2. visibility:hidden: 与上一个方法类似的效果，占据空间，但是不可以交互了
3. overflow:hidden: 这个只隐藏元素溢出的部分，但是占据空间且不可交互
4. display:none: 这个是彻底隐藏了元素，元素从文档流中消失，既不占据空间也不交互，也不影响布局
5. z-index:-9999: 原理是将层级放到底部，这样就被覆盖了，看起来隐藏了
6. transform: scale(0,0): 平面变换，将元素缩放为0，但是依然占据空间，但不可交互

## em\px\rem区别？
px：绝对单位，页面按精确像素展示。
em：相对单位，基准点为父节点字体的大小，如果自身定义了font-size按自身来计算（浏览器默认字体是16px），整个页面内1em不是一个固定的值。
rem：相对单位，可理解为”root em”, 相对根节点html的字体大小来计算，CSS3新加属性，chrome/firefox/IE9+支持

## 行内元素水平居中
1. 父元素设置 text-align:center,

## 块级元素水平居中的方式
1. margin:0 auto
```css
  .center{
      height: 200px;
      width:200px;
      margin:0 auto;
      border:1px solid red;
  }
  <div class="center">水平居中</div>
```

2. flex布局
```css
  .center{
      display:flex;
      justify-content:center;
  }
  <div class="center">
      <div class="flex-div">1</div>
      <div class="flex-div">2</div>
  </div>
```

3. table
```css
 .center{
      display:table;
      margin:0 auto;
      border:1px solid red;
  }
  <div class="center">水平居中</div>
```

4. 绝对定位 + transform
```
.son{
      position:absolute;
      left:50%;
      transform:translate(-50%,0);
}
```

5.  绝对定位 + 固定宽度 + left + margin-left

```son{
    position:absolute;
    width:固定;
    left:50%;
    margin-left:-0.5宽度;
}
```

6. 绝对定位 + 固定宽度 + left + margin
```
.son{
    position:absolute;
    width:固定;
    left:0;
    right:0;
    margin:0 auto;
}
```

## 行内元素垂直居中
 1. line-height 等于父元素高度
 
## 行内块级元素
1. vertical-align
```css
.parent::after, .son{
    display:inline-block;
    vertical-align:middle;
}
.parent::after{
    content:'';
    height:100%;
}
```
## 块级元素垂直居中

1. flex
```css
.parent {
  display: flex;
  align-items: center;
}
 
```

2. 绝对定位
```css
.son{
    position:absolute;
    top:50%;
    -webkit-transform: translate(0,-50%);  
    -ms-transform: translate(0,-50%);
    transform: translate(0,-50%);
}
```

3.  绝对定位 + 高度固定
```css
.son{
    position:absolute;
    top:50%;
    height:固定;
    margin-top:-0.5高度;
}
```
4.绝对定位 + 高度固定
 ```css
`.son{
    position:absolute;
    height:固定;
    top:0;
    bottom:0;
    margin:auto 0;
}
```


## css定位方式
1. static: 正常文档流定位

2. relative：相对定位，此时的『相对』是相对于正常文档流的位置。

3. absolute：相对于最近的非 static 定位祖先元素的偏移，

4. fixed：指定元素相对于屏幕视口（viewport）的位置来指定元素位置。

5. sticky：粘性定位，特性近似于relative和fixed的合体,须指定 top, right, bottom 或 left 四个阈值其中之一，
才可使粘性定位生效。否则其行为与相对定位相同。元素定位表现为在跨越特定阈值前为相对定位，之后为固定定位。


## Z-index

的z-index属性控制重叠元素的垂直叠加顺序，默认元素的z-index为0，而且z-index只能影响设置了position值的元素。


## 标准盒模型

W3C我们定义元素的width值即为盒模型中的content的宽度值，height值即为盒模型中的content的高度值。

## BFC？

一块独立的渲染的区域，让处于BFC内部的元素与外部的元素互相隔离。

1. BFC触发条件:

    根元素，即HTML元素
    position: fixed/absolute
    float 不为none
    overflow不为visible
    display的值为inline-block、table-cell、table-caption

## 为什么有时候用translate来改变位置而不是定位？

1.  改变transform或opacity不会触发浏览器重排（reflow）或重绘（repaint），只会触发复合（compositions）。
而改变绝对定位会触发重新布局，进而触发重绘和复合。
2.  transform使浏览器为元素创建一个 GPU 图层，但改变绝对定位会使用到 CPU。

## 伪类、伪元素？

伪类（pseudo-class） 是一个以冒号(:)作为前缀，被添加到一个选择器末尾的关键字，当你希望样式在特定状态下才被呈现到指定的元素时，你可以往元素的选择器后面加上对应的伪类。
伪元素用于创建一些不在文档树中的元素，并为其添加样式。
伪类是通过在元素选择器上加入伪类改变元素状态，而伪元素通过对元素的操作进行对元素的改变。

## flex
item的属性：
order:  属性定义项目的排列顺序。数值越小，排列越靠前，
flex-grow： 属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。
flex-shrink： 属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。为0不缩小
flex-basis：属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。
flex： flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。
align-self： 属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。


## 重排，重绘
重排发生的根本原理就是元素的几何属性发生了改变
1. 添加或删除可见的DOM元素
2. 元素位置改变
3. 元素本身的尺寸发生改变
4. 内容改变
5. 页面渲染器初始化
6. 浏览器窗口大小发生改变

重绘只会改变vidibility、outline、背景色等属性导致样式的变化，使浏览器需要根据新的属性进行绘制
重排的开销远大于重绘


## 从输入URl开始
https://segmentfault.com/a/1190000013662126

## animation 动画 与 transition过渡 和优化

### animation
基础属性值：

    animation-name: 动画名称(默认值为none)
    animation-duration: 持续时间(默认值为0)
    animation-timing-function: 时间函数(默认值为ease)
    animation-delay: 延迟时间(默认值为0)
    animation-iteration-count: 循环次数(默认值为1)
    animation-direction: 动画方向(默认值为normal)
    animation-play-state: 播放状态(默认值为running)
    animation-fill-mode: 填充模式(默认值为none)
    
 关键帧
 
 @keyframes animation-name{
    from | 0%{}
    n%{}
    to | 100%{}
}
```css
@keyframes test{
    100%{background-color: black;}
    60%{background-color: lightgray;}
    30%{background-color: lightgreen;}
    0%{background-color: lightblue;}
}
div{
    width: 300px;
    height: 100px;
    background-color: pink;
    animation-name: test;
    animation-duration: 3s;
    animation-iteration-count: infinite;
}
```

### transition

也是一个复合属性： 
1. transition-property: 过渡属性(默认值为all)
2. transition-duration: 过渡持续时间(默认值为0s)
3. transiton-timing-function: 过渡函数(默认值为ease函数)
4. transition-delay: 过渡延迟时间(默认值为0s)














































































