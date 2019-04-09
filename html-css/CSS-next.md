# cssnext

除了之前介绍的[CSS-var](https://wxxcarl.github.io/note/html-css/CSS-var)，还有其它一些新的css属性，这里列举比较实用的几个

### css @apply
类似scss的@include,目前(2019.4)没有浏览器能直接支持，需要额外loader，[postcss-next](https://github.com/MoOx/postcss-cssnext),

        :root {
            --flex-basic: {
            display: flex;
            flex-wrap: no-wrap;
            }
            --flex-horizontal-btw: {
            @apply --flex-basic;
            flex-direction: row;
            justify-content: space-between;
            align-items: center;
            }
        }
        .page {
            position: absolute;
            @apply --flex-horizontal-btw;

        }


### css :matches

兼容性不是很好，需要加浏览器前缀，建议使用postcss

        div:matches(::before, .items) {
            color: red;
        }
        ===>
        div::before, div.items {
            color: red
        }

### CSS image-set

兼容性一般，详见 [caniuse](https://www.caniuse.com/#search=image-set)

        .foo {
            background-image: image-set(url(img/test.png) 1x,
                                        url(img/test-2x.png) 2x,
                                        url(my-img-print.png) 600dpi);
        }