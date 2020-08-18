# Css布局-圣杯布局
在做前端的知识点总结时，发现Css布局牵连的知识点竟然如此的多，难怪布局会成为前端面试的重点。以接下来要介绍的圣杯布局为例，所涉及的知识点有```float布局```、```margin负值```、```相对定位```、```bfc```、```clearfix```等。
## 核心思想
圣杯布局用来实现PC端的的三栏布局
- 中间区域作为内容主体，需要被优先的的加载和渲染
- 两侧区域内容的宽度固定，中间区域内容宽度自适应
- ```padding```为两侧区域预留出空间
- ```float布局```
- ```margin负值```，让两侧区域与中间区域横向重叠
## 初步搭建框架
---
>以左右两侧区域都为```200px```，中间区域自适应为例子

- HTML代码如下
```html
<div class="header">header</div>
<div class="container">
  <div class="main">main</div>
  <div class="left">left</div>
  <div class="right">right</div>
</div>
<div class="footer">footer</div>
```
- Css代码如下
```Css
*{
    margin: 0;
    padding:0;
}
.header{
  width: 100%;
  background-color: cadetblue;
  text-align: center;
}
.container{
  padding: 0 200px 0 200px;
}
.main{
    width: 100%;
    background: chocolate;
    text-align: center;
    float: left;
}
.left{
    width: 200px;
    background-color: darkgoldenrod;
    text-align: center;
}
.right{
    width: 200px;
    background-color: darkgreen;
    text-align: center;
}
.footer{
  width: 100%;
  background-color: burlywood;
  text-align: center;
}

```
- 此时效果图
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cb5dd17172b940c696ae934dd234e5f5~tplv-k3u1fbpfcp-zoom-1.image)
## 调整左侧区域的位置
---
> ```margin-top```、```margin-left```为负值时，元素会向上、向左移动
>
> ```margin-right```、```margin-bottom```为负值时，右侧元素左移、下侧元素上移
>
> 可以结合```盒模型```的角度进一步的理解产生这些效果的原因

- 将左侧区域的样式改为
```Css
.left{
    width: 200px;
    float: left;
    background-color: darkgoldenrod;
    margin-left: -100%;
    /* position: relative;
    right: 200px; */
    text-align: center;
}
```
>通过```margin-left:-100%```结合```float:left```偏移到该位置

- 效果为
![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1895c3b15ae449ba91c153772ee387b6~tplv-k3u1fbpfcp-zoom-1.image)

>再通过相对定位```postion:relative```、```right:200px```偏移到目标位置

- 效果为
![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8b4d55a255f54d86a115c97e68f83464~tplv-k3u1fbpfcp-zoom-1.image)

## 调整右侧区域位置
---
```Css
.right{
    width: 200px;
    float: left;
    background-color: darkgreen;
    margin-right: -200px;
    text-align: center;
}
```
> 右侧区域通过```margin-right:-200px```结合```float:left```可偏移到目标区域

- 效果为
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fe016e3a1d9941cbbc3db2738184d724~tplv-k3u1fbpfcp-zoom-1.image)

## 清除浮动
---
> 看看此时包裹main、left、right区域的父容器container
> ![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/248b8f2152d64977b481f937f1439319~tplv-k3u1fbpfcp-zoom-1.image)
> 因为子元素的浮动，高度为0

- 通过让父容器触发```BFC```解决
```Css
.container{
    padding: 0 200px 0 200px;
    overflow: auto;
}
```

- 手写clearfix清除浮动
```Css
.clearfix::after{
    content: '';
    display: block;
    clear: both;
}
```
```html
<div class="header">header</div>
<div class="container clearfix">
    <div class="main">main</div>
    <div class="left">left</div>
    <div class="right">right</div>
</div>
<div class="footer">footer</div>
```
- 效果为
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/41a6b0ef335b40e0975a2931e6b57a1a~tplv-k3u1fbpfcp-zoom-1.image)

## 谈谈最小宽度设置问题
---
- 当页面宽度小于600px时，页面会发生明显的错位
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9909d77d486348a690f8a2b123a1a356~tplv-k3u1fbpfcp-zoom-1.image)
- 看看此时```left```区域未使用```相对定位```的位置
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/37198305f21e41588f5f536bfb9a8db1~tplv-k3u1fbpfcp-zoom-1.image)
此时```container```容器的```content```小于```200px```，导致右边容器的```margin-left:-100%```小于```left```自身的宽度```200px```，```left```区域无法通过```margin-left负值```与```float:left```到上面，因为```margin-left```此时偏移的距离不够
![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/92956bd336044a3a990819602da039a0~tplv-k3u1fbpfcp-zoom-1.image)
所以，要让两侧区域与中间区域横向重叠要满足```left```区域能够通过```margin-left```偏移超过自身宽度的大小```200px```，即```container```区域的宽度始终要 ```大于等于``` ```200px```才能让两侧区域与中间区域横向重叠。
>计算页面最小宽度为 ```padding-left(200px)``` + ```padding-right(200px)``` + ```left区域(200px)``` = ```600px```

```Css
body{
    min-width: 600px;
}
```
- 效果
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/42706a8166c847c9a9daa5c445f030be~tplv-k3u1fbpfcp-zoom-1.image)