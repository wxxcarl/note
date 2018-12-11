
###vee-validate使用教程

*\*本文适合有一定Vue2.0基础的同学参考，根据项目的实际情况来使用，关于Vue的使用不做多余解释。本人也是一边学习一边使用，如果错误之处敬请批评指出\**
###一、安装
`npm install vee-validate@next --save`
*注意：@next,不然是Vue1.0版本*

`bower install vee-validate#2.0.0-beta.13 --save`

###二、引用
    import Vue from 'vue';
    import VeeValidate from 'vee-validate';
    Vue.use(VeeValidate);
####组件设置:
    import VeeValidate, { Validator } from 'vee-validate';
    import messages from 'assets/js/zh_CN';
    Validator.updateDictionary({
        zh_CN: {
            messages
        }
    });
    const config = {
        errorBagName: 'errors', // change if property conflicts.
        delay: 0,
        locale: 'zh_CN',
        messages: null,
        strict: true
    };
    Vue.use(VeeValidate,config);
*assets/js/zh_CN 代表你存放语言包的目录，从node_modules/vee-validate/dist/locale目录下面拷贝到你的项目
Validator还有更多应用，下面再讲。
config其它参数，delay代表输入多少ms之后进行校验，messages代表自定义校验信息，strict=true代表没有设置规则的表单不进行校验，errorBagName属于高级应用，自定义errors，待研究*

###三、基础使用
    <div class="column is-12">
        <label class="label" for="email">Email</label>
        <p class="control">
            <input v-validate data-rules="required|email" :class="{'input': true, 'is-danger': errors.has('email') }" name="email" type="text" placeholder="Email">
            <span v-show="errors.has('email')" class="help is-danger">{{ errors.first('email') }}</span>
        </p>
    </div>
*提醒：错误信息里面的名称通常就是表单的name属性，除非是通过Vue实例传递进来的。*
*提醒：养成好习惯，给每个field添加`name`属性，如果没有`name`属性又没有对值进行绑定的话，validator可能不会对其进行正确的校验*

####关于`errors`：
上面的代码我们看到有`errors.has`,`errors.first`,errors是组件内置的一个数据模型，用来存储和处理错误信息，可以调用以下方法：
- `errors.first('field')` - 获取关于当前field的第一个错误信息
- `collect('field')` - 获取关于当前field的所有错误信息(list)
- `has('field')` - 当前filed是否有错误(true/false)
- `all()` - 当前表单所有错误(list)
- `any()` - 当前表单是否有任何错误(true/false)
- `add(String field, String msg)` - 添加错误
- `clear()` - 清除错误
- `count()` - 错误数量
- `remove(String field) ` - 清除指定filed的所有错误

####关于`Validator`
Validator是以`$validator`被组件自动注入到Vue实例的。同时也可以独立的进行调用，用来手动检查表单是否合法,以传入一个对象的方式，遍历其中指定的field。

    import { Validator } from 'vee-validate';
    const validator = new Validator({
        email: 'required|email',
        name: 'required|alpha|min:3',
    }); 
    // or Validator.create({ ... });
你也可以在构造了validator之后设置对象参数

    import { Validator } from 'vee-validate';
    const validator = new Validator();
    
    validator.attach('email', 'required|email'); // attach field.
    validator.attach('name', 'required|alpha', 'Full Name'); // attach field with display name for error generation.
    
    validator.detach('email'); // you can also detach fields.
    
最后你也可以直接传值给field，测试是否可以通过校验，像这样：

    validator.validate('email', 'foo@bar.com'); // true
    validator.validate('email', 'foo@bar'); // false
    //或者同时校验多个:
    validator.validateAll({
        email: 'foo@bar.com',
        name: 'John Snow'
    });
    //只要有一个校验失败了，就返回false

###四、内置的校验规则
- `after{target}` - 比target要大的一个合法日期，格式(DD/MM/YYYY)
- `alpha` - 只包含英文字符
- `alpha_dash` - 可以包含英文、数字、下划线、破折号
- `alpha_num` - 可以包含英文和数字
- `before:{target}` - 和after相反
- `between:{min},{max}` - 在min和max之间的数字
- `confirmed:{target}` - 必须和target一样
- `date_between:{min,max}` - 日期在min和max之间
- `date_format:{format}` - 合法的format格式化日期
- `decimal:{decimals?}` - 数字，而且是decimals进制
- `digits:{length}` - 长度为length的数字
- `dimensions:{width},{height}` - 符合宽高规定的图片
- `email` - 不解释
- `ext:[extensions]` - 后缀名
- `image` - 图片
- `in:[list]` - 包含在数组list内的值
- `ip` - ipv4地址
- `max:{length}` - 最大长度为length的字符
- `mimes:[list]` - 文件类型
- `min` - max相反
- `mot_in` - in相反
- `numeric` - 只允许数字
- `regex:{pattern}` - 值必须符合正则pattern
- `required` - 不解释
- `size:{kb}` - 文件大小不超过
- `url:{domain?}` - (指定域名的)url

###五、自定义校验规则
#####1.直接定义
    const validator = (value, args) => {
        // Return a Boolean or a Promise.
    }
    //最基本的形式，只返回布尔值或者Promise，带默认的错误提示
#####2.对象形式
    const validator = {
        getMessage(field, args) { // 添加到默认的英文错误消息里面
            // Returns a message.
        },
        validate(value, args) {
            // Returns a Boolean or a Promise.
        }
    };
#####3.添加到指定语言的错误消息
    const validator = {
        messages: {
            en: (field, args) => {
                // 英文错误提示
            },
            cn: (field, args) => {
                // 中文错误提示
            }
        },
        validate(value, args) {
            // Returns a Boolean or a Promise.
        }
    };

创建了规则之后，用extend方法添加到Validator里面

    import { Validator } from 'vee-validate';
    const isMobile = {
        messages: {
            en:(field, args) => field + '必须是11位手机号码',
        },
        validate: (value, args) => {
           return value.length == 11 && /^((13|14|15|17|18)[0-9]{1}\d{8})$/.test(value)
        }
    }
    Validator.extend('mobile', isMobile);
    
    //或者直接
    
    Validator.extend('mobile', {
        messages: {
          en:field => field + '必须是11位手机号码',
        },
        validate: value => {
          return value.length == 11 && /^((13|14|15|17|18)[0-9]{1}\d{8})$/.test(value)
        }
    });
    
然后接可以直接把mobile当成内置规则使用了：

    <input v-validate data-rules="required|mobile" :class="{'input': true, 'is-danger': errors.has('mobile') }" name="mobile" type="text" placeholder="Mobile">
    <span v-show="errors.has('mobile')" class="help is-danger">{{ errors.first('mobile') }}</span>

#####4.自定义内置规则的错误信息

    import { Validator } from 'vee-validate';

    const dictionary = {
        en: {
            messages: {
                alpha: () => 'Some English Message'
            }
        },
        cn: {
            messages: {
                alpha: () => '对alpha规则的错误定义中文描述'
            }
        }
    };
    
    Validator.updateDictionary(dictionary);
    
暂时介绍到这里，应该已经上手了，有空再继续翻译。
其它问题或者更高级应用，请参考官方文档[Vee-Validate][1]


  [1]: http://vee-validate.logaretm.com/ 