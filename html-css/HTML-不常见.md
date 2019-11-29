## 一、DNS 预解析缓存
```html
<link rel="dns-prefetch" href="http://cdn.staticfile.org/">
```
## 二、资源预加载

1. 预加载
```html
<link rel="prefetch" href="http://blog.wpjam.com/" />
<link rel="prefetch alternate stylesheet" href="mozspecific.css" />
```
2.预渲染
```html
<link rel="prerender" href="http://blog.wpjam.com/" />
// 兼容
<link rel="prefetch prerender" href="http://blog.wpjam.com" />
```
## 三、Download 属性

直接下载并重命名，而不是浏览器打开
```html
<a href="downloadpdf.php" download="download.pdf">点击直接下载并保存成 download.pdf 文件</a>
```


<style>
    .page-header {
        display: none;
    }
</style>