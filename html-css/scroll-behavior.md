# scroll-behavior

## CSS 

#### 凡是浏览器需要滚动的地方都加一句`scroll-behavior:smooth`就好了！

在PC浏览器中，网页默认滚动是在`<html>`标签上的，移动端大多数在`<body>`标签上，于是，我加上这么一句：

    html, body { scroll-behavior:smooth; }

`scroll-behavior` 的浏览器兼容性不是很好，但这就像一个不要钱的免费抽奖，没有中奖，没关系，又没什么损失，中奖了自然好，锦上添花

> 点击页面末尾的“回到顶部”进行体验，真·实时效果！

**Ctrl + F 查找页面内容的时候也会生效，可能会导致页面滚动较慢，具体效果可自行体验,*

## JS

DOM元素的scrollIntoView()方法是一个IE6浏览器也支持的原生JS API，可以让元素进入视区，通过触发滚动容器的定位实现。

    target.scrollIntoView({
        behavior: "smooth"
    });

如，滚动到第一个a链接所在的位置：

    document.links[0].scrollIntoView({
        behavior: "smooth"
    });

如果我们的网页已经通过CSS设置了`scroll-behavior:smooth`声明，则我们直接执行`target.scrollIntoView()`方法就会有平滑滚动，无需再额外设置behavior参数

<a href="#">回到顶部</a>

<style>
    html, body { scroll-behavior:smooth; }
    .page-header {
        display: none;
    }
</style>