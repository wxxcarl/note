# JS操作文件

## ArrayBuffer
ArrayBuffer 对象，代表内存之中的一段二进制数据，可以通过「视图」进行操作。「视图」部署了数组接口，这意味着，可以用数组的方法操作内存。

创建一个指定长度的 ArrayBuffer 对象，并读取该对象的字节大小：
````
// 8 代表所需要分配的内存大小（单位：字节）
const buffer = new ArrayBuffer(8);

console.log(buffer.byteLength);
// 8
````
但是 `ArrayBuffer` 对象只读，是不能被直接操作的，只能通过「视图实例对象」来操作，它们会将缓冲区中的数据表示为特定的格式，并通过这些格式来读写缓冲区的内容。

而所谓的「视图实例对象」，也就是「类型数组对象」（TypedArray对象） 或 DataView 对象。

两者的区别在于，后者可以自定义格式和字节序。例如，前者只能实例化同样格式（比如都是 `Unit8Array` 类型）的对象，而后者可以第一个字节为 Unit8 类型，第二个字节为 Int16 类型，等等。后者一般不常用

## TypedArray

TypedArray 视图类型很特殊，它 ***不是一个构造函数，而是一组构造函数的统称***，也就是说不能在程序中直接使用 TypedArray 来实例化对象，因为它只属于一个「概念」。包含如下构造函数：
- Int8Array: 8位有符号整数，长度1个字节
- Uint8Array：8位无符号整数，长度1个字节
- Uint8ClampedArray：8位无符号整数，长度1个字节，溢出处理不同
- Int16Array：16位有符号整数，长度2个字节
- Uint16Array：16位无符号整数，长度2个字节
- Int32Array：32位有符号整数，长度4个字节
- Uint32Array：32位无符号整数，长度4个字节
- Float32Array：32位浮点数，长度4个字节
- Float64Array：64位浮点数，长度8个字节

实例化对象可以有如下写法：
````
// 写法 1：实例化一个空的视图对象
new Uint8Array();

// 写法 2：创建初始化为 0 的，包含 2 个元素的无符号整型数组
new Uint8Array(2);

// 写法 3：从 ArrayBuffer 中创建
// uint8Buff 和 uint16Buff 视图对象底层对应的是同一段内存对象
// 一方修改，会影响到另一方
const buff = new ArrayBuffer(8);
const uint8Buff = new Uint8Array(buff);
const uint16Buff = new Uint16Array(buff);
console.log(uint8Buff.length);
// 8
````
普通数组的操作方法和属性，对 TypedArray 类型数组完全适用。

## ArrayBuffer 与字符串的互相转换

通过 `String.prototype.charCodeAt `与 `String.fromCharCode` 来实现一下

````
function ab2str (arraybuffer) {
    return String.fromCharCode.apply(
        null,
        new Uint16Array(arraybuffer)
    );
}

function str2ab (str) {
    // UTF-16 编码中，一个字符在内存中需要占用两个字节
    var arraybuffer = new ArrayBuffer(str.length * 2);
    var u16Arr = new Uint16Array(arraybuffer);
    var len = u16Arr.length;

    for (var i=0; i<len; i++) {
        u16Arr[i] = str.charCodeAt(i);
    }
    return u16Arr;
}

console.log(
    ab2str( str2ab('Hello World') )
);
````

## 二进制数组的应用
验证下使用 ArrayBuffer 方式读取出来的文件内容是否正确

````
const fileInputer = document.getElementById('fileInputer');

function ab2str (arraybuffer) {
    return String.fromCharCode.apply(
        null,
        // 注意换成了 Uint8Array
        // 因为 txt 文档在电脑上一般都是 UTF-8 编码
        new Uint8Array(arraybuffer)
    );
}

fileInputer.addEventListener('change', function (event) {
    const target = event.target;
    if (!target.files[0]) {
        return ;
    }

    const file = evt.target.files[0];
    const reader1 = new FileReader();
    const reader2 = new FileReader();

    // 以 ArrayBuffer 方式读取，获取结果后转成字符串
    reader1.onloadend = function (evt) {
        console.log(
            reader1.result
        );
        console.log(
            ab2str(new Uint8Array(reader1.result))
        );
    };
    reader1.readAsArrayBuffer(file);

    // 以 text 方式读取，获取结果后直接展示
    reader2.onloadend = function (evt) {
        console.log(
            reader2.result
        );
    };
    reader2.readAsText(file);
}, false);
````
`readAsText`方法会读取整个文件的内容，会严重浪费性能，甚至导致浏览器卡死


<style>
    .page-header {
        display: none;
    }
</style>