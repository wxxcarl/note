## 一、input 新type

- email：在表单提交时提供了格式验证功能，要求@左右各有一个字符即可

- number：呈现一个数字输入框，在提交时会验证数字格式，使用min、max、step指定最小值、最大值、步长

- url：提供了URL验证输入框，只要满足字母+冒号即可

- tel：提供了电话号码输入框，在手机浏览器中会弹出数字模拟键盘

- search：显示一个搜索框，在手机浏览器中会在模拟键盘右下角显示“搜索”按钮

- range：显示为一个滑块组件(数值选择组件)，使用min、max、step指定最小值、最大值、步长

        <input type="range" min="0" max="100" step="2" value="60">
    
    <input type="range" min="0" max="100" step="2" value="60">

- color：显示一个颜色选择器

        <input type="color"/> 

    <input type="color"/>

- date：显示一个日期选择器

        <input type="date"/>

    <input type="date"/>

- month：显示一个月份选择器

        <input type="month"/>

    <input type="month"/>

- week：显示一个星期选择器

        <input type="week"/>

    <input type="week"/>

*题外： 密码表单自动填充，chrome浏览器会默认添加一个底色，且无法覆盖，可以用内投影来覆盖。`-webkit-box-shadow: 0 0 0px 100px white inset;`*




## 二、新的标签
以下属性仅用于展示数据，不会被表单提交
1. #### datalist：数据列表

可用于为另一个输入框提供静态的输入建议

        <datalist id="my-list">
            <option>aaaa</option>
            <option>bbbb</option>
            <option>cccc</option>
            <option>eeee</option>
        </datalist>

        <input type="text" list="my-list"> 

<datalist id="my-list">
    <option>aaaa</option>
    <option>bbbb</option>
    <option>cccc</option>
    <option>eeee</option>
</datalist>
<input type="text" list="my-list">

----------
2. #### progress：进度条
用于显示某个过程的进度信息，若不指定value，则左右无限滚动；若指定了value，则固定为指定的值

        <progress min="0" max="1" value="0.5"></progress>
        <progress min="0" max="1"></progress>

<progress min="0" max="1" value="0.5"></progress>
<progress min="0" max="1"></progress>

--------
3. #### meter
通过不同的颜色显示指定的数值在哪个范围“最优”

- min：最小值 
- max：最大值
- low：合理值中最低限度 
- high: 合理值中最高限度 
- optimum:最合适的值 
- value：当前值 

        <meter min="0" max="100" low="30" high="70" optimum="50" value="60"></meter>
        <meter min="0" max="100" low="30" high="70" optimum="50" value="90"></meter>
        <meter min="0" max="100" low="30" high="70" optimum="50" value="10"></meter>

<meter min="0" max="100" low="30" high="70" optimum="50" value="60"></meter>
<meter min="0" max="100" low="30" high="70" optimum="50" value="90"></meter>
<meter min="0" max="100" low="30" high="70" optimum="50" value="10"></meter>

4. #### output：输出
语义标签, 用于存放计算结果，外观效果与普通的span一样

        小计：<output>￥30.00</output>

## 三、 新增表单属性
- `autofocus` 自动获得焦点
- `autocomplete` 是否启用自动补全
- `multiple` 用于指定input type="file/email"可以有多个输入项，使用英文逗号分隔
- `form` 用于指定表单外部元素从属于哪个表单

        <form id="f1"></form>
        <input form="f1">

- `pattern` 限定当前输入域中必须输入符合指定正则表达式的值

        <input name="phone" pattern="1[3578]\d{9}">

## 四、validity校验

        <form action="">
            用户名: <input type="text" id="user" required/>
            <input type="submit" value="提交"/>
        </form>
        
        <script>
            var user =document.getElementById("user");
            user.oninvalid = function(e){
                console.log(e.target.validity.valid)
            };
            if(user.checkValidity() === false) {
                alert(user.validationMessage)
            }
            user.setCustomValidity("自定义错误信息");
        </script>

validity属性
- customError: 是否设置了自定义的 validity 信息
- patternMismatch:  匹配它的模式属性
- rangeOverflow: 值大于设置的最大值
- rangeUnderflow: 值小于它的最小值
- stepMismatch: 不是按规定的 step 属性设置值
- tooLong:  值超过了 maxLength 属性
- tooShort:  值小于 minLength 属性
- typeMismatch:  值不是预期相匹配的类型
- valid: 值是否合法
- valueMissing:  元素没有值






<style>
    .page-header {
        display: none;
    }
</style>