# Canvas vs SVG

真正对这两者产生兴趣，是在做数据可视化的过程中，通常有两种普遍选择，highcharts 和 echarts，他们分别用SVG和canvas来实现，看起来效果差不多，实际上两者的区别还是巨大的。

### SVG：

SVG 是一种使用 XML 描述 2D 图形的语言，历史悠久，2003年就成为W3C标准<br>
SVG 基于 XML，这意味着 SVG DOM 中的每个元素都是可用的。您可以为某个元素附加 JavaScript 事件处理器。<br>
在 SVG 中，每个被绘制的图形均被视为对象。如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。

### Canvas：

Canvas 通过 JavaScript 来绘制 2D 图形，HTML5提供的新元素，`<canvas>`只是用来定义图形所在的空间。<br>
Canvas 是逐像素进行渲染的。<br>
在 canvas 中，一旦图形被绘制完成，它就不会继续得到浏览器的关注。如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象

### 功能对比

| Canvas | SVG |
|-----|----|
|基于像素|基于形状(图形元素)|
|依赖分辨率|不依赖分辨率,矢量|
|功能简单，2D绘图|功能丰富，滤镜，形状，动画等|
|不支持事件处理器|支持事件处理器|
|只能通过script修改|script和css都可以修改|

### 性能对比

![canvas vs svg 性能](http://aicaistatic.oss-cn-hangzhou.aliyuncs.com/s/img/201904/22115502544.webp)
<center>canvas vs svg 性能(来源microsoft开发社区)</center>

### 结论(适用场景)

从功能和性能对比可以看出
- canvas 基于像素，能对每个点进行操作，更好的支持小范围内的复杂场景，适合图像密集型的游戏，其中的许多对象会被频繁重绘。但是文本渲染能力较弱
- SVG 复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）,适合带有大型渲染区域的应用程序（比如谷歌地图）,适合静态图片展示，高保真文档查看和打印的应用场景

### JS库

- [two.js](https://github.com/jonobr1/two.js)(适合炫酷的二维动画)
- [d3.js](https://github.com/d3/d3)(适合数据可视化)


<style>
    .page-header {
        display: none;
    }
</style>