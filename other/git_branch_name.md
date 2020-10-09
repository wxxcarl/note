# Git分支备注

### 背景
当项目比较庞大，迭代次数较多的时候，本地的开发分支往往按照一定的规则命名，比如日期，任务编号等，久而久之开发人员自己也压根不记得这个分支是干嘛的，切换的时候容易搞错，也不敢贸然删除。这时候给分支加一个备注就很有必要，让每个分支的功能一目了然。

### 实现
这里需要用到一个小工具，[git-br](https://github.com/bahmutov/git-branches)，原理很简单，就是给分支设置一个`description`属性，然后读取分支的时候同样去读取该属性，显示在分支名字后面。

1. 安装git-br

    ```js
    npm i -g git-br
    ````
2. 设置备注

    ```js
    git config branch.this_is_my_branch.description myBranchDescrition

    // example
    git config branch.dev.description 开发分支
    ````
3. 显示备注

    ```js
    git branch
    // or if you set alias
    git br
    ````

### 简化

如果你觉得`git config branch.XXX.description` 命令太难记或者懒得敲打键盘，同样可以为他设置alias

1. 打开git配置编辑器

    ```js
    git config --global  -e
    ````
    > `--golbal` 某个用户生效，位于项目下的`~/.gitconfig`<br>
    `--local` 某个项目生效，位于`.git/config`<br>
    `--system` 整个系统生效，位于 `/etc/gitconfig`


2. 编辑alias,在`[alias]`下增加配置。*注意，以下配置不要用 `git config` 的方式设置*

    ```js 
    [alias]
        //其它alias
        desc = "!f() { currBr=`git symbolic-ref -q --short HEAD`;git config branch.${currBr}.description $1; }; f"
    ````
3. 使用

    ```js 
    // 把当前分支的备注给为'开发分支',注意是“当前分支”
    git desc 开发分支
    ````

<style>
    .page-header {
        display: none;
    }
</style>