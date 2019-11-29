# ES6-ES10

## ES6

- 块级作用域const与let

- ### class

    `extends`实际上是在其父类的prototype上添加 `[[Prototype]]`，引用到父类的prototype
    
    `extends` 后接的不限于指定一个类，可以是返回一个类的表达式

    子类重新定义了父类的方法会被优先调用，但通常我们不想完全替代父方法，而是在父方法的基础上调整或扩展其功能，`super`的作用在于此:

    - 使用 super.method(...) 调用父方法。
    - 使用 super(...) 调用父构造函数（仅在 constructor 函数中）
    - constructor中必须先调用`super`才会有`this`

    class 语法也支持静态属性的继承。原理也是通过`prototype`

    内置类没有静态 `[[Prototype]] `引用。例如，Object 具有 `Object.defineProperty`，`Object.keys`等方法，但 Array，Date 不会继承它们。

    更详细内容请参考 [ES6 Class 继承与 super](https://segmentfault.com/a/1190000015565616)

- ### Module
- 导出
    ````js
    export var name = 'wxxcarl'
    export const sqrt = Math.sqrt
    export {name, sqrt}
    export function myModule(someArg) {
        return someArg;
    }  
    ````
- 导入

    ```js
    import {myModule} from 'myModule'
    import {name,sqrt} from 'test'
    ```

- 与`export default`的区别

1. 一个文件中，`export`可以多个，`export default`仅有一个

2. 通过`export`方式导出，在导入时要加`{ }`，`export default`则不需要

-  一条import 语句可以同时导入默认函数和其它变量。
    `import defaultMethod, { otherMethod } from 'xxx.js'`

- ### Promise
    ```js
    var waitSecond = new Promise(function(resolve, reject) {
        setTimeout(resolve, 1000);
    })

    waitSecond.then(function() {
        console.log("Hello"); // 1秒后输出"Hello"
        return waitSecond;  // ！！注意！！
    }).then(function(){
        console.log("Hi"); // 2秒后输出"Hi"
    })
    ```

- ### 箭头函数

    `=>`不只是`function`的语法糖，它还带来了其它好处。箭头函数与包围它的代码共享同一个`this`,能帮你很好的解决`this`的指向问题

- ### 参数默认值

    参数默认值不仅能使代码变得更加简洁而且能规避一些问题,比如参数为`0`或者`''`时导致的判断问题

- ### 模板字符串

    不仅能替换变量，而且能保持换行输出

- ### 解构和延展操作符

    可以设置默认值:
    ```js
    var a, b;
    [a=5, b=7] = [1];
    console.log(a); // 1
    console.log(b); // 7
    ```

ES6中只支持数组的延展操作符，ES7中增加了对象的支持

    
其他具体使用方法可以参考[阮一峰-ES6入门之解构赋值](http://es6.ruanyifeng.com/#docs/destructuring)

- ### 对象属性简写
    ```js
    const name='Ming',age='18',city='mail';
    const student = {
        name,
        age,
        sex
    }
    ```
- ### Symbol

- ### Set (WeakSet) 和 Map (WeakMap) 数据结构

    - `Set` 类似数组，但是值唯一，有以下实例属性和方法：
        - constructor 构造函数，默认就是Set函数
        - size 属性
        - add(value)
        - delete(value)
        - has(value)
        - clear() 清除所有成员，没有返回值

    - `WeakSet` 成员只能是对象，而且对象都是弱引用，垃圾回收机制不考虑WeakSet对该对象的引用
    - `Map` 类似对象，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键，有以下实例属性和方法：
        - size 属性
        - set(key, value) 
        - get(key) 
        - has(key) 
        - delete(key) 
        - clear() 清除所有成员，没有返回值
    - `WeakMap` 只接受对象作为键名（null除外）,而且键名所指向的对象，不计入垃圾回收机制

- ### Generator函数
    ```js
    function* helloWorldGenerator() {
        yield 'hello';
        yield 'world';
        return 'ending';
    }

    var hw = helloWorldGenerator();

    hw.next()
    // { value: 'hello', done: false }

    hw.next()
    // { value: 'world', done: false }

    hw.next()
    // { value: 'ending', done: true }

    hw.next()
    // { value: undefined, done: true }
    ```
- ### Reflect 对象
    ```js
    Reflect.apply(target, thisArg, args)
    Reflect.construct(target, args)
    Reflect.get(target, name, receiver)
    Reflect.set(target, name, value, receiver)
    Reflect.defineProperty(target, name, desc)
    Reflect.deleteProperty(target, name)
    Reflect.has(target, name)
    Reflect.ownKeys(target)
    Reflect.isExtensible(target)
    Reflect.preventExtensions(target)
    Reflect.getOwnPropertyDescriptor(target, name)
    Reflect.getPrototypeOf(target)
    Reflect.setPrototypeOf(target, prototype)
    ```
- ### Proxy 对象

    - `get(target, propKey, receiver)`：拦截对象属性的读取，比如proxy.foo和proxy['foo']。
    - `set(target, propKey, value, receiver)`：拦截对象属性的设置，比如proxy.foo = v或proxy['foo'] = v，返回一个布尔值。
    - `has(target, propKey)`：拦截propKey in proxy的操作，返回一个布尔值。
    - `deleteProperty(target, propKey)`：拦截delete proxy[propKey]的操作，返回一个布尔值。
    - `ownKeys(target)`：拦截Object.getOwnPropertyNames(proxy)、Object.getOwnPropertySymbols(proxy)、Object.keys(proxy)、for...in循环，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而Object.keys()的返回结果仅包括目标对象自身的可遍历属性。
    - `getOwnPropertyDescriptor(target, propKey)`：拦截Object.getOwnPropertyDescriptor(proxy, propKey)，返回属性的描述对象。
    - `defineProperty(target, propKey, propDesc)`：拦截Object.defineProperty(proxy, propKey, propDesc）、Object.defineProperties(proxy, propDescs)，返回一个布尔值。
    - `preventExtensions(target)`：拦截Object.preventExtensions(proxy)，返回一个布尔值。
    - `getPrototypeOf(target)`：拦截Object.getPrototypeOf(proxy)，返回一个对象。
    - `isExtensible(target)`：拦截Object.isExtensible(proxy)，返回一个布尔值。
    - `setPrototypeOf(target, proto)`：拦截Object.setPrototypeOf(proxy, proto)，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截。
    - `apply(target, object, args)`：拦截 Proxy 实例作为函数调用的操作，比如proxy(...args)、proxy.call(object, ...args)、proxy.apply(...)。
    - `construct(target, args)`：拦截 Proxy 实例作为构造函数调用的操作，比如new proxy(...args)。
    
- ### Array.from()

    将一个类数组对象或者可遍历对象转换成一个真正的数组.

    可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理

- ### Array.prototype 方法
     
     - find()
     - findIndex()
     - copyWithin()

## ES7

- ### Array.prototype.includes()

- ### 指数操作符`**`
    ```js
    2**2 === Math.pow(2,2)
    ```
## ES8

- ### async/await

    Generator 函数的语法糖
    ```js
    const asyncReadFile = async function () {
        const f1 = await readFile('/etc/fstab');
        const f2 = await readFile('/etc/shells');
        console.log(f1.toString());
        console.log(f2.toString());
    };
    ```
- ### Object.values() 和 Object.entries()

- ### String padding

    - `String.padStart(targetLength,[padString])`

    - `String.padEnd(targetLength,padString])`

- ### 函数参数列表结尾允许逗号

    主要作用是方便使用git进行多人协作开发时修改同一个函数减少不必要的行变更。

- ### Object.getOwnPropertyDescriptors()

    用来获取一个对象的所有自身属性的描述符,如果没有任何自身属性，则返回空对象。

- ### SharedArrayBuffer对象

    用来表示一个通用的，固定长度的原始二进制数据缓冲区，类似于 ArrayBuffer 对象，它们都可以用来在共享内存（shared memory）上创建视图。与 ArrayBuffer 不同的是，SharedArrayBuffer 不能被分离。

- ### Atomics对象

    Atomics 对象提供了一组静态方法用来对 SharedArrayBuffer 对象进行原子操作。

## ES9

- ### 异步迭代
    ```js
    async function process(array) {
        for await (let i of array) {
            doSomething(i);
        }
    }
    ```
- ### Promise.finally()

    指定Promise的最终逻辑，无论成功失败

- ### 正则表达式命名捕获组

    ES2018允许命名捕获组使用符号?<name>，在打开捕获括号(后立即命名
    ```js
    const reDate = /(?<year>[0-9]{4})-(?<month>[0-9]{2})-(?<day>[0-9]{2})/,
    match  = reDate.exec('2018-04-30'),
    year   = match.groups.year,  // 2018
    month  = match.groups.month, // 04
    day    = match.groups.day;   // 30
    ```
    任何匹配失败的命名组都将返回undefined。
    
    命名捕获也可以使用在replace()方法中：
    ```js
    const reDate = /(?<year>[0-9]{4})-(?<month>[0-9]{2})-(?<day>[0-9]{2})/,
    d      = '2018-04-30',
    usDate = d.replace(reDat, '$<month>-$<day>-$<year>')
    ```
- ### 正则表达式反向断言

    - 肯定反向断言, 非数字\D必须存在 `(?<=\D)`
    - 否定反向断言，表示一个值必须不存在  `(?<!\D)`
    
- ### 正则表达式dotAll模式

    正则表达式中点.匹配除回车外的任何单字符，标记s改变这种行为，允许行终止符的出现，例如：
    ````js
    /hello.world/.test('hello\nworld');  // false
    /hello.world/s.test('hello\nworld'); // true
    ````
## ES10

- ### Array的`flat()`方法和`flatMap()`方法

    - `flat`，字面意思，数组扁平化，附带功能，去除数组的空项
    - `flatMap`,首先使用映射函数映射每个元素，然后将结果压缩成一个新数组,效率比map更高

- ### String的`trimStart()`方法和`trimEnd()`方法

- ### Object.fromEntries()

    `Object.entries()` 的逆向操作
    ```js
    const map = new Map([ ['foo', 'bar'], ['baz', 42] ])
    const obj = Object.fromEntries(map)
    console.log(obj) // { foo: "bar", baz: 42 }
    ```
- ### Symbol.prototype.description

    获取Symbol类型数据的描述

- ### String.prototype.matchAll

    可以用来替换 `regexp.exec` 或者 regexp 的`/g`标志,该方法返回一个迭代器，需配合`for...of`, `array spread`, or `Array.from()` 实现功能
    ```js
    const regexp = RegExp('foo*','g'); 
    const str = 'table football, foosball';
    let matches = str.matchAll(regexp);

    Array.from(matches, m => m[0]);
    // Array [ "foo", "foo" ]
    ```
- ### 其他，都是提案阶段，或者使用场景太小，暂不列举

<style>
    .page-header {
        display: none;
    }
</style>