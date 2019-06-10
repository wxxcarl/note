# CSS计数器

*CSS计数器只能跟content属性在一起的时候才有作用*

### counter-reset

计数器命名, 指定起始值，默认0

    .count{
        counter-reset: checkNumber 1;
    }

*如设置为小数，Chrome向下取整，否则无效，使用面默认值*

### counter-increment

设置递增规则，默认`+1`

    .count{
        counter-reset: checkNumber 1;
        counter-increment: checkNumber 2;
    }

`counter-increment`定义几次就执行几次，后面可以跟多个计数器，计数规则可以是负数，实现减法。

### counter()

显示计数

    .show-count:after{
        content: counter(checkNumber)
    }

第二个参数可以指定为`list-style-type`，不一定是阿拉伯数字

    content: counter(checkNumber, lower-roman)

    /*list-style-type:disc | circle | square | decimal | lower-roman | upper-roman | lower-alpha | upper-alpha | none | armenian | cjk-ideographic | georgian | lower-greek | hebrew | hiragana | hiragana-iroha | katakana | katakana-iroha | lower-latin | upper-latin*/

### counters()

子序列，`counters(checkNumber, '-')`,

*详情参考[张鑫旭-counter计数器详解](https://www.zhangxinxu.com/wordpress/2014/08/css-counters-automatic-number-content/)*