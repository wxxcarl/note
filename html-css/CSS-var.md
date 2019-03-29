# CSS variable

## 兼容性
css原生变量，在17年初的时候面向大众，其实在16年的时候已经得到几乎所有主流的现代浏览器的支持，详细情况可以查看[caniuse][1]，其实它的统计数据稍微落后，比如我尝试了最新的QQ Browser，也已经支持了。
所以如果是面向内部的系统，可以放心大胆的使用了，反正我已经在多个系统中使用过，异常方便。如果对兼容性有顾虑，可以尝试这个[polyfill][2]

## 使用
css variable(后面简称var)的使用其实非常简单

#### 基本使用
    
    body{
      --dangerColor: #f00;
      color:var(--dangerColor)
    }
先用`--`定义一个变量，然后在作用域范围内(例子为body范围)，使用`var()`来引用变量即可。也可以使用`:root`把变量定义在全局：

    :root{
      --dangerColor: #f00;
    }
    body{
      color:var(--dangerColor)
    }
聪明的读者或许已经发现了什么，他有作用域（姑且称之为作用域）！对，我可以在任意作用域内定义它，使之在范围内有效，或者在不同作用域内重复定义，使之对应不同的属性值，这是毫无问题的。

#### 其它使用

    color: var(--dangerColor, #f00);          
    /* 如果没找到--dangerColor，就用'#f00' */
    
    color: var(--dangerColor, var(--redColor, #f00)); 
    /* 可以嵌套定义 */
    
    width: calc(var(--my-width) + 20px);
    /* 可以进行计算 */

在JS中的使用
    
    element.style.getPropertyValue("--dangerColor");
    element.style.setProperty("--dangerColor", "#f00");
    // var并不是严格意义的变量，你可以把它就当做一个html属性

## 语法问题

var的语法确实有点丑，相比较而言Sass的变量语法更容易让人接受，但是该语法是经过开发者们慎重考虑之后做出的决定，主要目的是为了与css语法兼容，并且考虑到以后的扩展性，而`"$foo"`语法在将来或有更加合适的用武之地。[查看详情][3]

## 与Sass变量的区别
相信很多人在接触到var之前，先学习了sass语法的变量，并且觉得很好用，事实也确实如此。那么为什么又要推出原生的css变量呢？事实上，原生的CSS变量并不是试图复制SASS等预处理器早已经实现的功能，而是为了实现预处理器不能实现的功能。

#### 1. Sass是预处理，不是实时的
    
    $gutter: 1em; 
    @media (min-width: 30em) { 
        $gutter: 2em; 
    } 
    .Container { padding: $gutter; }
    
结果： .Container { padding: 1em; }，媒体查询丢失。

由于是预处理的关系，在编译的时候无法查看DOM结构，所以媒体查询内的变量定义就会失效。而var实时生效，可以完美解决这个问题。

    :root {
        --gutter: 1em; 
    }
    
    @media (min-width: 30em) {
        :root {
            --gutter: 2em;
        }
        /* 定义的时候需要指定范围，root或者容器内 */
    }
    .Container { padding: var(--gutter); }

除了`@media`，类似的例子如下：

    $font-size: 1em; 
    .user-setting-large-text { 
        $font-size: 1.5em; 
    } 
    body { font-size: $font-size; }
    
#### 2. 预处理器变量不能继承
    
    .alert { background-color: lightyellow; } 
    .alert.info { background-color: yellow; } 
    .alert button { border-color: darken(background-color, 25%); }

最后一句声明试图在`<button>`元素从父元素`.alert`继承的background-color属性使用Sass的`darken`函数,当`.alert`加上`.info`的时候，`<button>`能据此作出响应。在预处理的环境下，显然这行不通。
而var和普通css一样，具有继承性，即：给元素定义之后，会把该属性传递给子元素，并且和子元素其它的继承属性同样作用在子元素上。

#### 3. 预处理器变量不可互操作
跨不同的工具集或CDN上托管的第三方样式表共享预处理器变量是不可能（或至少不容易）的。
而var是普通的CSS属性，可以与任何CSS预处理器一起使用。

## 总结
var为CSS带来了一套新的动态的、强大的功能，我相信最大的优势还有待发掘。

自定义属性填补了预处理器变量不能填补的空白。尽管如此，在许多情况下预处理器变量仍是易用与优雅的选择。正因如此，我坚信未来许多网站会**结合使用**两者。自定义属性用于动态主题，预处理器变量用于静态模板。


#### 参考文献
- [我为什么对原生CSS变量感到兴奋][4]


  [1]: https://www.caniuse.com/#search=css%20var
  [2]: https://github.com/jhildenbiddle/css-vars-ponyfill
  [3]: https://www.xanthir.com/blog/b4KT0
  [4]: https://www.w3cplus.com/css3/why-im-excited-about-native-css-variables.html


<style>
    .page-header {
        display: none;
    }
</style>