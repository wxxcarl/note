# activity拆分总结

## 入口

考虑到现有活动的访问路径，本次采用多入口的形式，并非spa。而vite的入口文件为index.html,通过:
```html
<script type="module" src="./main.js"></script>
```
从而访问真正的入口文件`main.js`

- 开发环境:
相对根目录直接访问index.html相应的目录，注意!!后面带上’/’,比如：`http://localhost:8091/cms/renderPage/`,另外，这种情况本地开发无法使用history路由模式，请使用hash:

    ```js
    mode: import.meta.env.DEV ? 'hash' : 'history',
    ````

    文件目录为：

    ```json
    activity-front
        --cms
        ----renderPage
        ------index.html
        ------index.vue
        ------main.js
        --vite.config.js
    ````

- 生产环境: 
直接访问index.html相应的目录,root设置为根目录

## appolo配置

多重方案供参考，本次使用第一种<br>
接口 /app/sys/front ， (分支feature_20210517_ZX-3074-frontParam)

#### 1.、初始化之前注入根实例
- 先定义一组默认配置，确保就算接口异常也能正常打开页面
- 用服务端返回的配置覆盖默认配置之后，初始化到vue根实例($root)
- 正常挂载vue，页面中使用配置的地方用`this.$root.X`取数
- libs等目录内去除相关appolo的配置，用其它方式实现。libs本就应该是工具类和基础类，不应和业务配置耦合
```js
Vue.prototype.$get('/app/sys/front').then(res => {
    const config = res.success ? res.result : {}
    Object.assign(globalProperties, config)
}).finally(() => {
    new Vue({
        el: '#app',
        data: globalProperties,
        render: h => h(Index)
    })
})
```

#### 2.、注入windows

#### 3.、动态加载，和页面初始化玻剥离

## nginx配置
本次为实现无缝迁移(域名和地址不变)，特意使用多入口打包的模式，并且在原有的`m.zhaojiling.com`的配置下对ngixn进行了一些特殊的配置:

```node
// 所有静态文件指向新的目录,默认静态目录为'assets',
// 使用'assets-activity'为区分以后别的项目拆分，单独指向
location ~ ^/assets-activity/.*  {
    root   /mnt/dat1/shuyao/activity-static-h5;
}

// activity-2020-main 采用spa模式，尽量不做修改，
// 但是要特殊指定一下路劲查找的位置
location ~ ^/activity/activity-2020-main/.* {
    root /mnt/dat1/shuyao/activity-static-h5;
    try_files $uri $uri/ /activity/activity-2020-main/index/index.html;
}

// activity/activity-开头和cms/renderPage 开头的活动，指定到打包根目录
location ~ ^/(activity/activity-|cms/renderPage).* {
    root /mnt/dat1/shuyao/activity-static-h5;
    index index.html;
}

// 原有配置，其它所有的资源(接口，页面，静态资源)继续指向原来的服务
location ~ {
    proxy_pass      http://cpsH5ApiServer;
}
````

注意： 路径需要独一无二区分，不能是`/activity/`或者`/cms/`，因为这些同时也是部分接口的前缀

## 弯路和坑

1. 上述的访问路径问题，appolo配置注入，nginx配置问题，每个都是坑，建议自己实现一遍，获益良多

2. 关于配置`cssCodeSplit`,自定义样式少，建议关闭它，css打包在一个文件。但问题是legacy插件不支持，开启兼容模式之后，所有的样式丢失，已提issue，感觉快要被修复

3. 还是legacy插件，1.4.0版本不支持自定义模块名称chunkFileNames(webpackChunkName)，1.4.1版本不支持函数模式的的模块名称，所以本次用的1.4.0，兼容模式下使用默认的文件名，ESM支持自定义。已提issue，感觉修复遥遥无期。后期不修复可以考虑计划放弃chunkFileNames自定义，这会给排查js报错问题带来一定的阻力

4. vite-plugin-vue2升级一个小版本(v1.5.2)居然放弃了`vue-template-compiler`依赖，导致报错，要手动安装

5. 通过观察issue以及自身体验，vite以及周边依赖，小版本升级也可能导致项目崩溃，而且目前更新很快，遇到好几次开发环境没问题上线就挂的，因为依赖版本不一致。所以，特别注意要提交package-lock.json文件，锁定版本号！
提交package-lock.json！
提交package-lock.json！
提交package-lock.json！

6. 其它记不清的各种问题，想起来了随时分享

<style>
    .page-header {
        display: none;
    }
</style>