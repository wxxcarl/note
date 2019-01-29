# grid

*本文并非完全原创，参考了一些其他文章，列表在末尾，本人只是用自己比较容易理解的语言简单整理，列出常用的布局并且画出demo，供以后参考，如有侵权请及时告知*

## 先谈浏览器支持
已经2019年了，grid在2年前的这个时候，已经被许多浏览器提供了原生的、不加前缀的支持，现在的支持情况已经比较好了，看图：
![2019年1月grip浏览器支持][1]
  
  *如果你确实需要兼容旧版本浏览器，可以尝试[css-grid-polyfill][2]*
  
## 概念
- 二维布局: 同时指定横向和纵向布局的方式
- container（父元素），是实际的 grid(网格)
- item（子元素），grid(网格) 内的内容
- line(网格线)，就像`table-border`
- track，一行或者一列
- cell，一个单元格
- area，由任意4根首尾相连的网格线组成的封闭区域

## 简单示例

第一步当然是先设置容器

    <div class="wrapper">
      <div class="item1">1</div>
      <div class="item2">2</div>
      <div class="item3">3</div>
      <div class="item4">4</div>
      <div class="item5">5</div>
      <div class="item6">6</div>
    </div>
    .wrapper {
        display: grid;
    }

#### 简单布局：

    .wrapper {
        display: grid;
        grid-template-columns: 100px 100px 100px;
        grid-template-rows: 100px 100px;
    }

注释：*columns列宽,rows行高,此处设置子元素为3列2行,同样可以使用其它CSS单位如`%`,`rem`和`auto`,`fr`等，`1fr`的作用等同于`flex:1;`,按比例撑满自由空间。*

![此处输入图片的描述][3]

#### 个性化布局：
    
    //删除item6
    .wrapper {
        display: grid;
        grid-template-columns: 100px 100px 100px;
        grid-template-rows: 100px 100px 100px;
    }
    .item1 {
        grid-column:1/3;
    }
    .item2 {
        grid-row:1/3;
      	grid-column: 3/4;
    }
    .item3 {
        grid-row: 2/4;
    }
    .item5 {
        grid-column: 2/4;
    }
    
`grid-column:1/3` 为`grid-column-start: 1;`和 `grid-column-end: 3;`的缩写,表示从column第1根网格线到第3根网格线区域，以此类推，不指定则自动填充，`grid-row`同理。
没有指定结束行值(只有一个值)，则该网格项默认跨越1个轨道,也可以理解为放在该值对应的网格内。
可以用`[]`给网格线命名,可多个名字,如
    
    .wrapper {
        grid-template-rows: [line1] 100px [line2 line2-2] 100px [line3] 100px [line4]
    }
    .item3 {
        grid-row: line2 / line4;
    }
    
![此处输入图片的描述][4]

到了这里，相信你已经感受到grid的魅力了吧。

#### 进阶布局
    
    .wrapper {
        display: grid;
        grid-template-columns: 100px 100px 100px;
      	grid-template-rows: 100px 100px 100px;
      	grid-template-areas: 
        "header header header"
        "side center main"
        "footer footer footer";
    }
    .item1 {
        grid-area: header;
    }
    .item2 {
        grid-area: side;
    }
    .item3 {
        grid-area: center;
    }
    .item4 {
      grid-area: main;
    }
    .item5 {
        grid-area: footer;
    }

注释：*通过引用 `grid-area`属性指定的网格区域的名称来定义网格模板。 重复网格区域的名称导致内容扩展到这些单元格。 一个或连续多个中间不带空格的点号`.`都可以表示一个空单元格。 `grid-template-areas`语法本身提供了网格结构的可视化。*
以上`grid-template-columns`,`grid-template-rows`,`grid-template-areas`可以缩写为：

    grid-template:
    "header header header" 100px
    "side center main" 100px
    "footer footer footer" 100px
    / 100px 100px 100px;

    
![此处输入图片的描述][5]

####高级布局

    <div class="wrapper">
        <div class="key">aaaaaaaaaaaa</div>
        <div class="key">bbb</div>
        <div class="key">ccc</div>
        <div class="key">ddd</div>
      	<div class="value">AAA</div>
        <div class="value">BBB</div>
        <div class="value">CCC</div>
        <div class="value">DDD</div>
    </div>

------

    .wrapper {
        display: grid;
      	grid-template-columns: 50px 1fr;
      	grid-gap: 5px;
      	grid-auto-flow: dense;
    }
    .key{
      grid-column: 1;
      word-break:break-all;
    }

![此处输入图片的描述][6]

## 其它：

#### container
- grid-column-gap： 设置纵向网格线大小，即列之间的间距,
- grid-row-gap： 行之间的间距
- grid-gap: `<grid-row-gap> <grid-column-gap>`的缩写
- justify-items: 沿着行轴对齐网格内的内容（左右对齐）
- align-items: 沿着列轴对齐网格里的内容（上下对齐）
- justify-content/align-content: 整个grid在容器中左右/上下对齐方式
- grid-auto-columns/grid-auto-rows: items超出grip范围时，未被定义的网格的宽度/高度
- grid-auto-flow：未指定网格的items的自动填充方式，`row`按行依次填充，`column`按列，`dense`，尝试用后面的items填充空白空间
- grid-template: grid-template-columns,grid-template-rows,grid-template-areas的缩写
- grip: 所有以下属性的简写：grid-template-rows，grid-template-columns，grid-template-areas，grid-auto-rows，grid-auto-columns和grid-auto-flow,为了代码可读性，不建议这么写,grid-template足矣。

#### item
- grid-column-start / grid-column-end / grid-row-start /grid-row-end
    使用特定的网格线确定 grid item 在网格内的位置
    值：
    - `<line>`: 可以是一个数字来指代相应编号的网格线，也可使用名称指代相应命名的网格线
    - `span <number>`: 网格项将跨越指定数量的网格轨道
    - `span <name>`: 网格项将跨越一些轨道，直到碰到指定命名的网格线
    - `auto`: 自动布局， 或者自动跨越， 或者跨越一个默认的轨道
- grid-column / grid-row
    grid-column-start + grid-column-end, 和 grid-row-start + grid-row-end 的简写形式。
- justify-self/align-self： 沿着行（列）轴对齐单个网格内的内容


## 其它等有空继续整理

- 打开chrome调试工具查看grid会展示设置的网格，一目了然

## 参考列表

- [5分钟学会 CSS Grid 布局][7]
- [CSS Grid 系列(上)-Grid布局完整指南][8]
- [用 grid 布局轻松解决 flex 布局不太好做的一个问题][10]


  [1]: http://aicaistatic.oss-cn-hangzhou.aliyuncs.com/s/img/201901/29104335654.jpg
  [2]: https://github.com/FremyCompany/css-grid-polyfill
  [3]: http://aicaistatic.oss-cn-hangzhou.aliyuncs.com/s/img/201901/29111230613.png
  [4]: http://aicaistatic.oss-cn-hangzhou.aliyuncs.com/s/img/201901/29112358917.png
  [5]: http://aicaistatic.oss-cn-hangzhou.aliyuncs.com/s/img/201901/29121918137.png
  [6]: http://aicaistatic.oss-cn-hangzhou.aliyuncs.com/s/img/201901/29152452039.png
  [7]: https://www.html.cn/archives/8506
  [8]: https://segmentfault.com/a/1190000012889793
  [9]: https://juejin.im/entry/5a2f458af265da432840d1fd
  [10]: https://blog.csdn.net/beijiyang999/article/details/80868095