# egg-validate的使用和定制化升级

## 基本使用

egg-validate基于[parameter](https://github.com/node-modules/parameter)，基本使用查看readme
#### 自带规则：
- 'int' => {type: 'int', required: true}
- 'int?' => {type: 'int', required: false }
- 'integer' => {type: 'integer', required: true}
- 'number' => {type: 'number', required: true}
- 'date' => {type: 'date', required: true}
- 'dateTime' => {type: 'dateTime', required: true}
- 'id' => {type: 'id', required: true}
- 'boolean' => {type: 'boolean', required: true}
- 'bool' => {type: 'bool', required: true}
- 'string' => {type: 'string', required: true, allowEmpty: false}
- 'string?' => {type: 'string', required: false, allowEmpty: true}
- 'email' => {type: 'email', required: true, allowEmpty: false, format: EMAIL_RE}
- 'password' => {type: 'password', required: true, allowEmpty: false, format: PASSWORD_RE, min: 6}
- 'object' => {type: 'object', required: true}
- 'array' => {type: 'array', required: true}
- [1, 2] => {type: 'enum', values: [1, 2]}
- /\d+/ => {type: 'string', required: true, allowEmpty: false, format: /\d+/}

#### 自定义校验规则

````js
 // 校验用户名是否正确
app.validator.addRule('userName', (rule, value)=>{
    if (/^\d+$/.test(value)) {
        return "用户名应该是字符串";
    } else if (value.length < 3 || value.length > 10) {
        console.log("用户名的长度应该在3-10之间");
    }
});
// 可以直接放在app/extend/application.js里面。return的就是错误消息message，但是error不止这个，还有错误字段等会自动包装好。
````

## 基于egg-validate的定制化升级

达到以下效果：
1. app.js里面少写一些代码，最好就写一两行，做个配置这样子
2. 对于所有的自定义校验规则独立出文件夹，可以取名validate，就丢在app/下面
3. 针对相似的校验规则进一步抽象成文件，就叫做user.js这样，丢在app/validate/下面
4. 针对某一条特定的校验规则，如校验用户的userName就丢在app/validate/user.js里面
5. 然后保持egg-validate的使用规则不变，原来是ctx.validate现在还是ctx.vallidate。同时其他的插件、配置不受影响。这叫做代码侵入性小。

#### 1. 首先把app.js里面导入模块写出来
````js
const path = require('path');

module.exports = app => {

  // 你的其它代码，balabala

  // 加载所有的校验规则
  const directory = path.join(app.config.baseDir, 'app/validate');
  // 使用Loader来加载validate下面的所有文件
  app.loader.loadToApp(directory, 'validate');
}
````

#### 2. 建立实际的校验规则文件

````js
// app/validate/user.js

module.exports = app =>{
  let { validator } = app;
  // 校验用户名是否正确
  validator.addRule('userName', (rule, value)=>{
    console.log(rule);
    if (/^\d+$/.test(value)) {
      return "用户名应该是字符串";
    }
    else if (value.length < 3 || value.length > 10) {
      console.log("用户名的长度应该在3-10之间");
    }
  });
  // 添加自定义参数校验规则
  validator.addRule('123', (rule, value) => {
    if (value !== '123'){
      return 'must be 123';
    }
  });
};
````


<style>
    .page-header {
        display: none;
    }
</style>