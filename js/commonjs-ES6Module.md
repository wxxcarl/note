# ES6 模块与 CommonJS 模块的差异

## 总览 
- CommonJS 输出是值的拷贝，即原来模块中的值改变不会影响已经加载的该值，ES6静态分析，动态引用，输出的是值的引用，值改变，引用也改变，即原来模块中的值改变则该加载的值也改变。
- CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。
- CommonJS 加载的是整个模块，即将所有的接口全部加载进来，ES6 可以单独加载其中的某个接口（方法），
- CommonJS this 指向当前模块，ES6 this 指向undefined

CommonJS 模块输出的是值的拷贝，也就是说，一旦输出一个值，模块内部的变化就影响不到这个值。ES6 模块的运行机制与 CommonJS 不一样。JS 引擎对脚本静态分析的时候，遇到模块加载命令import，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。ES6 模块不会缓存运行结果，而是动态地去被加载的模块取值，并且变量总是绑定其所在的模块。

CommonJs模块化：
```js
  // lib.js
  var counter = 3;
  function incCounter() {
  counter++;
  }
  module.exports = {
  counter: counter,
  incCounter: incCounter,
  };
  // main.js
  var mod = require('./lib');
  
  console.log(mod.counter);  // 3
  mod.incCounter();
  console.log(mod.counter); // 3
```

ES6模块化
```js
  // lib.js
  export let counter = 3;
  export function incCounter() {
  counter++;
  }
  
  // main.js
  import { counter, incCounter } from './lib';
  console.log(counter); // 3
  incCounter();
  console.log(counter); // 4
```
## 语法差异

### ES6 

#### export

```js
export var firstName = 'Michael';
export var lastName = 'Jackson';
export function multiply(x, y) {
  return x * y;
};
```
```js
var firstName = 'Michael';
var lastName = 'Jackson';
export { firstName, lastName as surname };
```

#### import 

```js
import { firstName, lastName as surname } from 'xx.js';
import * as Name from 'xx.js'
```
```js
foo();
// import命令具有提升效果，会提升到整个模块的头部，首先执行。
import { foo } from 'my_module';
```

#### export default

```js
// export-default.js
export default function () {
  console.log('foo');
}
// 一个模块只能有一个默认输出
```
```js
import customName from './export-default';
customName(); 
```

#### 混合

```js
// export.js
export default function (obj) {}
export function each(obj, iterator, context) {}
export { each as forEach };
```
```js
import _, { each, forEach } from 'export.js';
```

#### 循环依赖

ES6模块不会缓存运行结果，而是动态地去取被加载的模块值，以及变量总是绑定其所在的模块.

ES6根本不会关心是否发生了"循环加载"，只是生成一个指向被加载模块的引用，需要开发者自己保证，真正取值的时候能够取到值。

--------

## CommonJs

#### export
js
````
// 属性
var EventEmitter = require('events').EventEmitter;
module.exports = new EventEmitter();

setTimeout(function() {
  module.exports.emit('ready');
}, 1000);
````
````js
// 为了方便，Node为每个模块提供一个exports变量，指向module.exports
var exports = module.exports;
exports.area = function (r) {
  return Math.PI * r * r;
};

````

不能直接将exports变量指向一个值，因为这样等于切断了exports与module.exports的联系。

如果一个模块的对外接口，就是一个单一的值，不能使用exports输出，只能使用module.exports输出。

如果你觉得，exports与module.exports之间的区别很难分清，一个简单的处理方法，就是放弃使用exports，只使用module.exports。


#### require

require发现参数字符串指向一个目录以后，会自动查看该目录的package.json文件，然后加载main字段指定的入口文件。如果package.json文件没有main字段，或者根本就没有package.json文件，则会加载该目录下的index.js文件或index.node文件。

#### 循环依赖

一旦出现某个模块被"循环加载"，就只输出已经执行的部分，还未执行的部分不会输出。


<style>
    .page-header {
        display: none;
    }
</style>