# navigator.sendBeacon

`navigator.sendBeacon()` 方法可用于通过HTTP将少量数据通过`POST`且<b>`只能通过POST`</b>请求异步传输到Web服务器。

> 这个方法主要用于满足统计和诊断代码的需要，这些代码通常尝试在卸载（`unload`）文档之前向web服务器发送数据。过早的发送数据可能导致错过收集数据的机会。然而，对于开发者来说保证在文档卸载期间发送数据一直是一个困难。因为用户代理通常会忽略在 `unload` 事件处理器中产生的异步 `XMLHttpRequest。`

>这就是 `sendBeacon() `方法存在的意义。使用 `sendBeacon()` 方法会使用户代理在有机会时异步地向服务器发送数据，同时不会延迟页面的卸载或影响下一导航的载入性能。这就解决了提交分析数据时的所有的问题：数据可靠，传输异步并且不会影响下一页面的加载。此外，代码实际上还要比其他技术简单许多！

## 参数
`url `参数表明 data 将要被发送到的网络地址。

`data `参数是将要发送的 [ArrayBufferView](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypedArray) 或 [Blob](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob), [DOMString](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString) 或者 [FormData](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData) 类型的数据。

## 请起头

- 如果数据类型是 string，则可以直接上报，此时该请求会自动设置请求头的 Content-Type 为 `text/plain`

    ```js
    navigator.sendBeacon(url, data);
    ````

- 如果用 Blob 发送数据，这时需要我们手动设置 Blob 的 MIME type，一般设置为 `application/x-www-form-urlencoded`
    ```js
    const blob = new Blob([JSON.stringify(data)],{
        type: 'application/x-www-form-urlencoded',
    });
    navigator.sendBeacon(url, blob);
    ````

- 可以直接创建一个新的 Formdata，此时该请求会自动设置请求头的 Content-Type 为 `multipart/form-data`

    ```js
    const formData = new FormData();
    for (const [key, value] of Object.entries(data)) {
        formData.append(key, value)
    }
    navigator.sendBeacon(url, formData);
    ````
- json格式，也是创建blob对象，设置type为`application/json`
    ```js
    var data = {hello: "world"};
    var blob = new Blob([JSON.stringify(data)], {type : 'application/json'});
    navigator.sendBeacon(url, blob);
    ````

<style>
    .page-header {
        display: none;
    }
</style>