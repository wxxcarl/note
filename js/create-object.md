# Object.create() vs new Object()

## Object.create(), es6创建对象的方式
语法：Object.create(proto, [propertiesObject])
- proto : 必须。表示新建对象的原型对象，即该参数会被赋值到目标对象(即新对象，或说是最后返回的对象)的原型上。该参数可以是null， 对象， 函数的prototype属性 （创建空的对象时需传null , 否则会抛出TypeError异常）。
- propertiesObject : 可选。 添加到新创建对象的可枚举属性（即其自身的属性，而不是原型链上的枚举属性）对象的属性描述符以及相应的属性名称。这些属性对应[Object.defineProperties()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperties)的第二个参数。
如：
````
var obj = {};
Object.defineProperties(obj, {
'property1': {
    value: true,
    writable: true
},
'property2': {
    value: 'Hello',
    writable: false
}
// etc. etc.
});

// configurable, enumerable, writable 默认为false
````

## 区别

### 一、 创建对象的方式不同
- `new Object()` 通过构造函数来创建对象, 添加的属性是在自身实例下，修改的属性影响原始对象。
- `Object.create()` 可以理解为继承一个对象, 添加的属性是在原型下，修改的属性不影响原始对象。

````
// new Object() 方式创建
var a = {  rep : 'apple' }
var b = new Object(a)
console.log(b) // {rep: "apple"}
console.log(b.__proto__) // {}
console.log(b.rep) // "apple"
b.name='John'
console.log(a.name) // "John"
console.log(a === b)   // true

// Object.create() 方式创建
var a = { rep: 'apple' }
var b = Object.create(a)
console.log(b)  // {}
console.log(b.__proto__) // {rep: "apple"}
console.log(b.rep) // "apple"
b.name='John'
console.log(a.name) // undefined
````
### 二、 创建对象属性的性质不同
`Object.create()` 用第二个参数来创建非空对象的属性描述符默认是为false，而构造函数或字面量方法创建的对象属性的描述符默认为true.可使用`Object.getOwnPropertyDescriptors（）`验证

### 三、创建空对象时不同
当用构造函数或对象字面量方法创建空对象时，对象时有原型属性的，即有_proto_;
当用Object.create()方法创建空对象时，对象是没有原型属性的。

### 四、`__proto__` 属性

- `Object.create()`

创建的对象，属性和方法是添加在`__proto__`上的
````
var proto = {
    name: 'John',
    talk(){ }
};
var o = Object.create(proto);

console.log(o)
//    {}
//      __proto__:
//          name: "John"
//          talk: ƒ talk()
````

如果用构造函数实现，则需要设置prototype:
````
var People = function(){}
People.prototype.name = 'John'
People.prototype.talk = function() {}
var p = new People();
````

- `Object.setPrototypeOf`

作用与 `__proto__` 相同，用来设置一个对象的 prototype 对象，返回参数对象本身。它是 ES6 正式推荐的设置原型对象的方法。

````
var proto = {
    name: 'John',
    talk(){ }
};
var o = {age: 30}
Object.setPrototypeOf(o, proto);
console.log(o)
//    {}
//      age: 40
//      __proto__:
//          name: "John"
//          talk: ƒ talk()

````
- `Object.getPrototypeOf()`

读取一个对象的原型对象
````
Object.getPrototypeOf('foo') === String.prototype // true
Object.getPrototypeOf(true) === Boolean.prototype // true
````

### 五、 原型属性的继承
Object.assign 是不能拷贝到继承或原型上的属性和方法的
````
var o2 = Object.assign({}, o)
o2.name // undefined
````
- 方法一， getPrototypeOf()得到属性之后继承
````
var originProto = Object.getPrototypeOf(o);
var originProto2 = Object.create(originProto);
var o2 = Object.assign(originProto2, o);
````
- 方法二(推荐)
````
var originProto = Object.getPrototypeOf(o);
var originDesc = Object.getOwnPropertyDescriptors(o)
var o2 = Object.create(originProto, originDesc);
````
Object.getOwnPropertyDescriptors() 得到是 o 对象自身的可枚举属性，作为第二个参数，放在 o2 的实例上。

为什么说推荐这个方法呢？因为Object.assign() 方法不能正确拷贝 get ，set 属性,只会拷贝getter返回的结果,而Object.getOwnPropertyDescriptors()可以返回正确的getter和setter


<style>
    .page-header {
        display: none;
    }
</style>