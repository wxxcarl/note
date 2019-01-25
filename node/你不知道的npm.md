# 你不知道的npm

## 全部默认初始化
`npm init --yes`
## 自定义默认设置
在 Home 目录创建一个 .npm-init.js 即可,该文件的 module.exports 即为 package.json 配置内容。

例如编写如下 ~/.npm-init.js

    const desc = prompt('description?', 'xxx\'s project')

    module.exports = {
        key: 'my key',
        name: prompt('name?', process.cwd().split('/').pop()),
        version: '0.0.1',
        description: desc,
        main: './src/index.js',
        scripts: {
            "serve": "vue-cli-service serve",
            "build": "vue-cli-service build"
        },
        dependencies: {
            "vue": "^2.5.17"
        },
        devDependencies: {
            "@vue/cli-service": "^3.0.0"
        }
    }


除了生成 package.json可以添加默认dependencies, 因为 .npm-init.js 是一个常规的模块，意味着我们可以执行随便什么 node 脚本可以执行的任务。例如通过 fs 创建 README, .eslintrc 等项目必需文件，实现项目脚手架的作用(*注意使用严格的JSON格式，容易拼写错误*)。
##### **题外话，自己再测试的时候发现一个小技巧，`mkdir npm-test && cd &_`* 可以创建文件夹并且直接进入，比mkdir 然后 cd ，简单那么一丢丢，适合文件夹名字较复杂场景

## package的定义
`npm install <package>` 其中的package可以是以下任意一种情况

| #  | 说明                                   | 例子              |
|----|--------------------------------------|-----------------|
| a) | 一个包含了程序和描述该程序的 package.json 文件 的 文件夹 | ./local-module/ |
|b) |	一个包含了 (a) 的 gzip 压缩文件|	./module.tar.gz|
|c)|	一个可以下载得到 (b) 资源的 url (通常是 http(s) url)	| https://registry.npmjs.org/we... |
|d)|	一个格式为 <name>@<version> 的字符串，可指向 npm 源(通常是官方源 npmjs.org)上已发布的可访问 url，且该 url 满足条件 (c)	|webpack@4.1.0|
e)|	一个格式为 <name>@<tag> 的字符串，在 npm 源上该<tag>指向某 <version> 得到 <name>@<version>，后者满足条件 (d)|	webpack@latest
f)|	一个格式为 <name> 的字符串，默认添加 latest 标签所得到的 <name>@latest 满足条件 (e)	|webpack
g)|	一个 git url, 该 url 所指向的代码库满足条件 (a) |	git@github.com:webpack/webpack.git

上面表格的定义意味着，我们在共享依赖包时，并不是非要将包发表到 npm 源上才可以提供给使用者来安装。这对于私有的不方便 publish 到远程源（即使是私有源），或者需要对某官方源进行改造，但依然需要把包共享出去的场景来说非常实用。比如引用一個本地的xxx.js,只要放到xxx目录下，新建一个package.json文件，然后就可以在项目package.json内添加该dependencies`"xxx": "file:./xxx"`，或者直接`npm install file:./xxx`，避免引用路径过长



