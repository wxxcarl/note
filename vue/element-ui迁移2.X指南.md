1. Vue 版本需要最低更新到2.5.2
2. 没有了theme-default主题，新增了主题chalk，所以css引用路径由
 `import 'element-ui/lib/theme-default/index.css'`
 改为
 `import 'element-ui/lib/theme-chalk/index.css'`

3. button和steps等组件的icon属性需要传入完整的icon名称，比如
 `<el-button type="primary" icon="edit">编辑</el-button>`
 改为
 `<el-button type="primary" icon="el-icon-edit">编辑</el-button>`
 而input组件的icon，不能再使用icon="xxx"来实现，可以通过 prefix-icon 和 suffix-icon 属性在 input 组件首部和尾部增加显示图标，也可以通过 slot 来放置图标。并且移除 了on-icon-click 属性和 click 事件
4. 受vue版本影响，需要把scope换成了slop-scope
5. steps组件不能再使用`text-align:center`来居中，而要使用align-center属性。
6. Tabs 标签页和Pagination分页的样式有改变（我对其进行了自定义。一般情况不需要注意）
7. Collapse 折叠面板样式更改，个人觉得以前的更好，视场景而定
8. Checkbox组件的change事件回调参数发生了变化，1.x里面是event 事件对象，2.x里面是更新后的值。但是v-model绑定的不受影响
9. Menu组件移除了 theme 属性。现在通过 background-color、text-color 和 active-text-color 属性进行颜色的自定义
10. Ztree样式变动较大
11. el-table组件，在2.0.8之后的某一个版本进行了改动，动态生成column的table，v-if不能写在column上面，而要写在el-table标签上面。理解一下就是，有了el-table必须要有column，否则会导致浏览器假死崩溃，原因未知。
12. 更多变更请查看[更新日志](http://element-cn.eleme.io/2.0/#/zh-CN/component/changelog)




