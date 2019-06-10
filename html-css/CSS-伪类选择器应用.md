# CSS-伪类选择器应用

## 伪类匹配列表数目

````
li:only-child { /* 1个 */ }
li:first-child:nth-last-child(2) { /* 2个 */ }
li:first-child:nth-last-child(3) { /* 3个 */ }
````

在CSS中，伪类是可以级联使用的，于是，如果列表可以匹配`:first-child:nth-last-child(2)`则表示当前`<li>`元素即是第1个子元素，又是从后往前第2个子元素，因此，我们就能判断当前总共两个`<li>`子元素，我们就能精准实现我们想要的布局了，只需要配合相邻兄弟选择符加号`+`以及兄弟选择符弯弯`~`即可.例如：
```
/* 3个li项目的第1个列表项 */
li:first-child:nth-last-child(3) {}

/* 3个li项目的第1个列表项的后一个，也就是第2项的样式 */
li:first-child:nth-last-child(3) + li {}

/* 3个li项目的第一个列表项后面两个列表项，也就是第2项和第3项的样式 */
li:first-child:nth-last-child(3) ~ li {}
```

详情参考：[张鑫旭-伪类匹配列表数目实现微信群头像CSS布局的技巧](https://www.zhangxinxu.com/wordpress/2019/03/nth-last-child-css-layout/)


## `:default`伪类选择器简介

`:default`伪类选择器只能作用在表单元素上，表示默认状态的表单元素。

```
<select multiple>
    <option>选项1</option>
    <option>选项2</option>
    <option>选项3</option>
    <option selected>选项4</option>
    <option>选项5</option>
    <option>选项6</option>
</select>
```
```
option:default {
    color: red;
}
```
必须要设置`selected`或者`checked`才会生效，而不是第一个选项生效。IE不支持，Edge新版支持

