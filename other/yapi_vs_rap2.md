# YAPI VS RAP2

### 维护人员
- yapi 去哪儿前端团队维护.github stars：6k
- rap2 个人维护，issue内多次提到个人精力有限.github stars：3k

### 部署
- yapi 基于koa，react，mongodb，环境简单，只需要nodejs，git，mongodb，甚至支持可视化部署，启动一个服务，填写配置后自动完成
- rap2 虽然也基于koa和react，但是前后端分开部署，还要`mysql`和redis，亲测部署困难，错误不断.

### 迁移成本
- yapi 迁移成本低，多种方式导入导出
- rap2 只支持从rap1导入，导出只有postman

### 功能
- yapi 支持：ldap，自定义mock规则，期望mock，编写wiki，支持JSON-SCHEMA编写或者json5，自定义占位符，可配基本路径，支持严格模式(校验请求参数)，等等。。。。
- rap2 几乎和rap1一样的功能，只是用koa和react重构了一下，简化了界面（界面已经可以称之为简陋了，很不用心），值得一赞的是协同仓库功能，这在yapi内暂没找到替代方法，理论上可以通过自定义脚本支持，但是实现应该有点复杂

### 二次开发&插件
- yapi 文档相对齐全，提供丰富的开放api和钩子函数，可以方便的进行二次开发和组件开发，添加组件也很方便,在github上已经有很多的插件可以下载。例如本人的rap1数据导入插件 https://github.com/wxxcarl/yapi-plugin-import-rap ，已添加到官方插件列表
- rap2 文档非常简陋，api少，只能查询，对二次开发很不友好，插件方面甚至没有看到相关文档

### 测试
- 测试功能还没仔细研究，但是粗看，yapi测试功能很强大，有丰富的配置(环境，断言等)，rap2只看到有个测试的url，没找到文档所以不多说






<style>
    .page-header {
        display: none;
    }
</style>