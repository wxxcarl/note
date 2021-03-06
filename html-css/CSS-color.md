## CSS色彩之RGB

写css的时候用到颜色，以前个人使用RGB是最多的，反正基本上都是复制黏贴，也不需要太关注他代表的是什么颜色，如果需要自己定义颜色，就随便用一个代替，如`color: red;`, `color: #999`等。

以前看到一些色值的时候，对于他想表达的颜色，很难有概念，除非很常见的`#f00`等，对于`#18b481`等，就一脸懵逼了，还有`rgb(200, 50, 100)`这种表达方式也是类似。如果不了解其相关原理，就无法解读。（当然，现在的流行开发工具，基本上都能直接支持或者通过插件支持显示定义的颜色）。特意花了点时间了解一下，纪录在此。

RGB色彩的原理来自于三原色叠加理论，分别代表Red,Green,Blue，把六位十六进制数字按顺序分为 3 组，每两位一组分别代表R、G、B，'00'为没有色彩，'ff'代表满色彩，三组都为00，即为黑色，三组都为ff，则为白色，因此可以对三组数据分开解读，根据各自的颜色深浅，叠加之后进行判断大致的颜色。如扇面的`#18b481`,18代表红色较弱，b4代表绿色强，81代表蓝色较强，不难想象，最后呈现的颜色应该是绿色偏蓝。`rgb()`函数的方式原理是一样的，只是把16进制数换成了0-255的色彩值，我们可以根据之前的原理来写一个跟`#18b481`相近的颜色:`rgb(30, 180, 120)`。效果如下：

<div style="background: #18b481;color: #fff;">
    background: #18b481
</div>
<div style="background: rgb(30, 180, 120);color: #fff;">
    background: rgb(30, 180, 120)
</div>

rbg()还有一中表示方式，百分比，原理一样，即相对于255的百分比,如
<div style="background: rgb(20%, 70%, 50%);color: #fff;">
    background: rgb(20%, 70%, 50%)
</div>

## CSS色彩之HSL

RGB色彩对机器实现是友好的，因此也是电脑显示器显示彩色的主要实现原理，但是对人其实不够友好，通常来说一串纯数字是不太具有可读性的，还是16进制的。特别是需要定义一些颜色，在进行颜色计算的时候，就更不友好了，因此我们常见的需要变换颜色的地方，很多都是通过HSL来实现，它对人而言更加直观，更加人性化.

`hsl(hue, saturation, lightness)`

第一个参数代表色相，人眼所能感知的颜色范围，请看下图：

![色相圆](https://p3.ssl.qhimg.com/t01cc4bbeb17b549ecb.png)

以正上方为 0 度，顺时针为正方向，每个角度值代表一个颜色。几个特殊的角度值为：0° 红、60° 黄、120° 绿、180° 青、240° 蓝、300° 洋红,这个规律是不是很熟悉？加上30°的橙，不就是彩虹的颜色吗？所以太好记了，几乎无成本。此参数的单位为角度，deg 可以省略

第二个参数为色彩饱和度，这个值接受的为 0 ～ 100%的百分数。一般地，色彩的饱和度为 100%。此时颜色最为鲜艳。当降低颜色饱和度，则颜色的表现就不那么鲜艳了，直到 0%，接近灰色。可以简单理解为鲜艳程度，最不鲜艳的时候，就是灰白色。

第三个参数是亮度，默认值是 50%，当该参数为0时，完全没有亮度，即黑色，100%为白色。

示例：`color: hsl(120deg, 100% , 50%);` 代表最鲜艳的绿色

了解这三个参数之后，就能更方便的判断出大致的颜色的，而不需要去考虑什么三原色叠加理论了。如果需要颜色的转化，通常只需要改变第一个参数就行。


<style>
    .my-color-block2{
        color:#fff;
        background-color: rgb(30, 180, 120);
    }
</style>
<style>
    .page-header {
        display: none;
    }
</style>