# webpack3到4主要区别

- ### 自动设置process.env.NODE_EVN
    webpack增加了一个`mode`配置，只有两种值 `development` | `production`。对不同的环境他会启用不同的配置,不再使用`DefinePlugin`

- ### UglifyJsPlugin不再需要
    只需要配置`optimization.minimize = true`就行，而且production mode下面自动为true。

    optimization.minimizer可以配置你自己的压缩程序

- ### CommonsChunkPlugin废弃
    这是最主要的改动，wenpack4使用`optimization.splitChunks`配置进行模块划分

    #### `CommonsChunkPlugin`的缺点

    1. 如今web应用多为SPA，遇到的首要问题是，所有模块打包到一起之后，如何避免首次入口文件过大，导致首屏渲染速度大大降低。解决方法相比都知道，可以使用`import()`语法实现模块的懒加载,比如`vue-router`:

        const main = () => import(/* webpackChunkName: "main" */ '@/views/main/index.vue')

    2. 使用懒加载之后，各个模块志中的公共模块就会重复打包

    为了解决上述第二个问题，`CommonsChunkPlugin`应运而生，它能够将全部的懒加载模块引入的共用模块统一抽取出来，形成一个新的common块，但是这又回到了上述第一个问题，首屏需要加载的公共模块往往过大，导致渲染太慢。

    以前这就需要你在代码重复与入口文件控制方面做个平衡，而这个平衡挺不利于多人开发的，也不易于优化。（需要团队成员事先交流评估，哪些模块需要懒加载，哪些可以直接集成。另外，个人有个想法，就是在配置`CommonsChunkPlugin`的时候，指定抽取哪些组件里面的公共模块，理论上可行，但是团队配合比较麻烦。）

    `CommonsChunkPlugin`的另一个缺点是，配置复杂，各种配置多样化，新手往往看的一脸懵逼，就如官方推荐的配置：

        new webpack.optimize.CommonsChunkPlugin({
            name: 'vendor',
            minChunks (module) {
                // any required modules inside node_modules are extracted to vendor
                return (
                module.resource &&
                /\.js$/.test(module.resource) &&
                module.resource.indexOf(
                    path.join(__dirname, '../node_modules')
                ) === 0
                )
            }
        }),
        new webpack.optimize.CommonsChunkPlugin({
            name: 'manifest',
            minChunks: Infinity
        }),
        new webpack.optimize.CommonsChunkPlugin({
            name: 'app',
            async: 'vendor-async',
            children: true,
            minChunks: 3
        })

    很多人往往不明白为什么要这样配置三次(分别是为了提取第三方模块，运行模块和业务模块)，另外其他的各种文档还有各种不同的配置。

    #### `SplitChunksPlugin`

    `SplitChunksPlugin` 的出现就是为了解决上述问题。根据字面意思理解,“split”,切割，它能够抽出懒加载模块之间的公共模块，并且不会抽到父级，而是会与首次用到的懒加载模块并行加载，这样我们就可以放心的使用懒加载模块了。看如下例子：

        // 假设有以下4个模块
        chunk-a: react, react-dom, some components
        chunk-b: react, react-dom, some other components
        chunk-c: angular, some components
        chunk-d: angular, some other components

        // SplitChunksPlugin 处理后
        commonChunk1(for chunk-a, chunk-b): react, react-dom
        commonChunk2(for chunk-c, chunk-d): angular
        chunk-a to chunk-d: Only the components

    `SplitChunksPlugin`配置比较明确，使用官网默认配置基本可以满足大多数单页应用了

        optimization: {
            splitChunks: {
                chunks: "async",  // 必须三选一： "initial" | "all"(推荐) | "async" (默认就是async)
                minSize: 30000,  // 最小尺寸(压缩前的)
                minChunks: 1,  // 最小 chunk
                maxAsyncRequests: 5,  // 最大异步请求数
                maxInitialRequests: 3,  // 最大初始化请求书
                name: true, // 打包后的名称（true为根据chunks 和 cache group key自动起名）
                cacheGroups: {  置缓存的 chunks
                    default: {
                        minChunks: 2,  // 最小引用数
                        priority: -20  // 缓存组打包的先后优先级
                        reuseExistingChunk: true,  //是否重用该chunk
                    },
                    vendors: {
                        test: /[\\/]node_modules[\\/]/,  // 正则规则验证，如果符合就提取 chunk
                        priority: -10
                    }
                }
            }
        }

    详情访问[RIP CommonsChunkPlugin](https://gist.github.com/sokra/1522d586b8e5c0f5072d7565c2bee693)


- ### 番外 `externals`

    `externals`可以将第三方库以cdn的方式去引入

        // HTML
        <script src="https://code.jquery.com/jquery-3.1.0.js" crossorigin="anonymous">
        </script>

        // webpack
        externals: {
            jquery: 'jQuery', // window.jQuery
        }

        // modules
        import $ from 'jquery';
        $('.my-element').animate(/* ... */);