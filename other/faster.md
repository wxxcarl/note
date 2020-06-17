# 优化小结

## 项目优化

- 骨架屏 `vue-server-renderer`
- 预渲染 `prerender-spa-plugin`


## 代码优化

- 缩放图片高耗性能，img标签多大，图片就给多大
- dns-preftech dns预解析
- 尽量用事件委托，显著的提高事件的处理速度，减少内存的占用，动态的添加DOM元素，不需要因为元素的改动而修改事件绑定



## Vue优化



## 打包优化

- 大文件cdn引用, 配置`externals`
- vue-cli-service build --modern 
- 合理配置[prefetch](https://cli.vuejs.org/zh/guide/html-and-static-assets.html#prefetch)


<style>
    .page-header {
        display: none;
    }
</style>