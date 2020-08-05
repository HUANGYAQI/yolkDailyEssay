# JS基础

## 变量提升

JavaScript引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行一行地运行。
这造成的结果，就是所有的变量的声明语句，都会被提升到代码的头部，这就叫做变量提升（hoisting）。
`
console.log(a) // undefined
var a = 1
function b() {
    console.log(a)
}
b() // 1
`


## 闭包
闭包是函数和声明该函数的词法环境的组合。
闭包最大的作用就是隐藏变量，闭包的一大特性就是内部函数总是可以访问其所在的外部函数中声明的参数和变量，即使在其外部函数被返回（寿命终结）了之后
基于此特性，JavaScript可以实现私有变量、特权变量、储存变量等

```
(function() {
    var a = 1;
    function add() {
        var b = 2
        var sum = b + a
        console.log(sum); // 3
    }
    add()
})()
```

```js
function Person(){
    var name = 'cxk';
    this.getName = function(){
        return name;
    }
    this.setName = function(value){
        name = value;
    }
}

const cxk = new Person()

console.log(cxk.getName()) //cxk
cxk.setName('jntm')
console.log(cxk.getName()) //jntm
console.log(name) //name is not defined
```

## JavaScript的作用域链

JavaScript属于静态作用域，即声明的作用域是根据程序正文在编译时就确定的，有时也称为词法作用域。


## js类型
JavaScript的类型分为两大类，一类是原始类型，一类是复杂(引用）类型。

*  null
*  undefined
*  boolean
*  string
*  symbol
*  number

复杂类型
* object

可能还有个bigint

## bigint
JavaScript中Number.MAX_SAFE_INTEGER表示最大安全数字,计算结果是9007199254740991，即在这个数范围内不会出现精度丢失（小数除外）。
但是一旦超过这个范围，js就会出现计算不准确的情况，这在大数计算的时候不得不依靠一些第三方库进行解决，因此官方提出了BigInt来解决此问题。


## null与undefined的区别是什么
null表示为空，代表此处不应该有值的存在，一个对象可以是null，代表是个空对象，而null本身也是对象。
JavaScript是一门动态类型语言，成员除了表示存在的空值外，还有可能根本就不存在（因为存不存在只在运行期才知道），这就是undefined的意义所在。


## 原型链？

### 原型对象 
绝大部分的函数(少数内建函数除外)都有一个prototype属性,这个属性是原型对象用来创建新对象实例,
而所有被创建的对象都会共享原型对象,因此这些对象便可以访问原型对象的属性。

例如hasOwnProperty()方法存在于Obejct原型对象中,它便可以被任何对象当做自己的方法使用.

### 原型链的形成

原因是每个对象都有 __proto__ 属性，此属性指向该对象的构造函数的原型。
对象可以通过 __proto__与上游的构造函数的原型对象连接起来，而上游的原型对象也有一个__proto__，这样就形成了原型链。


## 判断数组的方式

1. Array.isArray(）
2. Object.prototype.toString.call(arg)==='[object Array]'
3. Object.getPrototypeOf() 方法返回指定对象的原型: Object.getPrototypeOf(arr) === Array.prototype; // true

## this （抬头望天） ? 
this的指向不是在编写时确定的,而是在执行时确定的，同时，this不同的指向在于遵循了一定的规则。

1. 在默认情况下，this是指向全局对象的window
```
name = "yolk";
function sayName () {
    console.log(this.name);
};
sayName(); //"yolk"
```

2. 总是代表着它的直接调用者,函数被调用的位置存在上下文对象时，那么函数是被隐式绑定的
```js
function f() {
    console.log( this.name );
}
let obj = {
    name: "yolk",
    f: f
};
obj.f(); //被调用的位置恰好被对象obj拥有，yolk

const obj = {
    num: 10,
   hello: function () {
    console.log(this);    // obj
    setTimeout(function () {
      console.log(this);    // window
    });
   }    
}
obj.hello();

```

3. 严格模式下（设置了'use strict'），this为undefined
```js
function hello() { 
   'use strict';
   console.log(this);  // undefined
}  
hello();
```

4. 当使用call，apply，bind（ES5新增）绑定的，this指向绑定对象
```js 
bind
-----------
function f() {
    console.log( this.name );
}
const obj = {
    name: "Messi",
};
const obj1 = {
     name: "Bale"
};
f.bind(obj)(); 



```

5. 箭头函数中this,默认指向定义它时，所处上下文的对象的this指向。即使是call，apply，bind等方法也不能改变箭头函数this的指向

```js
const obj = {
    num: 10,
   hello: function () {
    console.log(this);    // obj
    setTimeout(() => {
      console.log(this);    // obj
    });
   }    
}
obj.hello();

const obj = {
  radius: 10,  
  diameter() {    
      return this.radius * 2
  },  
  perimeter: () => 2 * Math.PI * this.radius
}
console.log(obj.diameter())    // 20
console.log(obj.perimeter())    // NaN

```

6. new一个新对象，this指向这个新对象。
```js
function Person(name) {
  this.name = name;
  console.log(name);
}
var person1 = new Person('Messi'); //Messi
```


## async/await
async 函数，就是 Generator 函数的语法糖，它建立在Promises上，并且与所有现有的基于Promise的API兼容。
Async—声明一个异步函数(async function someName(){...})
1. 自动将常规函数转换成Promise，返回值也是一个Promise对象
2. 只有async函数内部的异步操作执行完，才会执行then方法指定的回调函数
3. 异步函数内部可以使用await

Await—暂停异步的功能执行(const result = await someAsyncCall()
1. 放置在Promise调用之前，await强制其他代码等待，直到Promise完成并返回结果
2. 只能与Promise一起使用，不适用与回调
3. 只能在async函数内部使用


## async/await相比promise优势？
1. 代码读起来更加同步，Promise虽然摆脱了回调地狱，但是then的链式调用也会带来额外的阅读负担
2. Promise传递中间值非常麻烦，而async/await几乎是同步的写法，非常优雅
3. 错误处理友好，async/await可以用成熟的try/catch，Promise的错误捕获非常冗余
4. 调试友好，Promise的调试很差，由于没有代码块，你不能在一个返回表达式的箭头函数中设置断点，如果你在一个.then代码块中使用调试器的步进(step-over)功能，调试器并不会进入后续的.then代码块，因为调试器只能跟踪同步代码的『每一步』。


## 预解释和变量提升

```js
var a= 1;
function f() {
  console.log(a);
  var a = 2;
}
f();
```
1. 变量声明与赋值

其中以上例中的var a = 2;在JavaScript引擎处理时却分为了两个步骤:

    1. 读取var a后,在当前作用域中查找是否有相同声明,如果没有就在当前作用域集合中创建一个名为a的变量,否则忽略此声明继续进行解析.
    2. 接下来,V8引擎会处理a = 2的赋值操作,首先会询问当前作用域中是否有名为a的变量,如果有进行赋值,否则继续向上级作用域询问.

2. 函数声明与函数表达式

 f成功被打印出来,而g函数出现了类型错误,这是什么原因呢?
```js
f();
g();
//函数声明
function f() {
    console.log('f');
}
//函数表达式
var g = function() {
    console.log('g');
};
```


## Event Loop
在JavaScript中，任务被分为两种，一种宏任务（MacroTask）也叫Task，一种叫微任务（MicroTask）
MacroTask（宏任务）：script全部代码、setTimeout、setInterval、setImmediate（浏览器暂时不支持，只有IE10支持，具体可见MDN）、I/O、UI Rendering。
MicroTask（微任务）： Process.nextTick（Node独有）、Promise、Object.observe(废弃)、MutationObserver（具体使用方式查看这里）

JS调用栈：JS调用栈采用的是后进先出的规则，当函数执行的时候，会被添加到栈的顶部，当执行栈执行完成后，就会从栈顶移出，直到栈内被清空。


## 同步任务和异步任务
Javascript单线程任务被分为同步任务和异步任务，同步任务会在调用栈中按照顺序等待主线程依次执行，异步任务会在异步任务有了结果后，
将注册的回调函数放入任务队列中等待主线程空闲的时候（调用栈被清空），被读取到栈内等待主线程的执行。
任务队列Task Queue，即队列，是一种先进先出的一种数据结构。

## 事件循环的进程模型
执行栈在执行完同步任务后，查看执行栈是否为空，如果执行栈为空，就会去检查微任务(microTask)队列是否为空，如果为空的话，就执行Task（宏任务），否则就一次性执行完所有微任务。
每次单个宏任务执行完毕后，检查微任务(microTask)队列是否为空，如果不为空的话，会按照先入先出的规则全部执行完微任务(microTask)后，
设置微任务(microTask)队列为null，然后再执行宏任务，如此循环。













































































