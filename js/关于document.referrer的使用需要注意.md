# referrer

项目使用到一个场景，ajax请求返回无权限，跳回登录页面，登录后自动返回之前的浏览页，跳转由前端处理，于是想到document.referrer,但是对可靠性不确定，特意搜索了一下相关资料，大致整理如下，如有侵权，请告知删除。

- 只有当用户在上一个页面点击链接到达当前页面，或者location.href到达当前页面，document.referrer才会有值。
- 当用户输入这一页的网址、通过response.redirect、用了ssl、通过书签进入页面，这些情况referrer都会为空。
- 当网站使用refresh字段进行跳转的时候，大多数浏览器不发送referrer
- 从用户从一个HTTPS的网站点击链接到另一个HTTP的网站时，不发送referrer
- html5中，a标签的rel="noreferrer"可以让浏览器不发送referrer
- 使用Data+URI+scheme链接的，浏览器也不发送referrer
- 使用Content+Security+Policy,也可以让浏览器不发送referrer
- 在html头部中使用meta标签来控制不让浏览器发送referrer
- 当网站使用refresh字段进行跳转的时候，大多数浏览器不发送referrer




