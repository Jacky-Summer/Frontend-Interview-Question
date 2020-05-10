## 51.说说你对 AMD 和 Commonjs 的理解

CommonJS 是服务器端模块的规范，Node.js 采用了这个规范。CommonJS 规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。AMD 规范则是非同步加载模块，允许指定回调函数
AMD 推荐的风格通过返回一个对象做为模块对象，CommonJS 的风格通过对 module.exports 或 exports 的属性赋值来达到暴露模块对象的目的

## 52.用过哪些设计模式？

- 工厂模式：

工厂模式解决了重复实例化的问题，但还有一个问题,那就是识别问题，因为根本无法搞清楚它们到底是哪个对象的实例
主要好处就是可以消除对象间的耦合，通过使用工程方法而不是 new 关键字
创建多个相似对象的问题，我们可以用一个函数来封装它所有的属性，这样我们只要给函数传入不同的参数（属性值），就能轻松创建出一个对象。

```javascript
function person(a, b) {
  var o = {} //这个对象就相当于模子
  o.name = a
  o.getName = function () {
    console.log(this.name)
  }
  return o //函数被调用时就会返回这个对象
}
var fun = person('wanghan', 20)
```

- 构造函数模式
  使用构造函数的方法，即解决了重复实例化的问题，又解决了对象识别的问题，该模式与工厂模式的不同之处在于直接将属性和方法赋值给 this 对象

## 53.offsetWidth/offsetHeight,clientWidth/clientHeight 与 scrollWidth/scrollHeight 的区别

offsetWidth/offsetHeight 返回值包含 content + padding + border，效果与 e.getBoundingClientRect()相同
clientWidth/clientHeight 返回值只包含 content + padding，如果有滚动条，也不包含滚动条
scrollWidth/scrollHeight 返回值包含 content + padding + 溢出内容的尺寸

## 54.什么情况下会发生布尔值的隐式强制类型转换？

1. if (..) 语句中的条件判断表达式。
2. for ( .. ; .. ; .. ) 语句中的条件判断表达式（第二个）。while (..) 和 do..while(..) 循环中的条件判断表达式。
3. ? : 中的条件判断表达式。
4. 逻辑运算符 ||（逻辑或）和 &&（逻辑与）左边的操作数（作为条件判断表达式）。
5. || 和 && 操作符的返回值？

   || 和 && 首先会对第一个操作数执行条件判断，如果其不是布尔值就先进行 ToBoolean 强制类型转换，然后再执行条件
   判断。

对于 || 来说，如果条件判断结果为 true 就返回第一个操作数的值，如果为 false 就返回第二个操作数的值。

&& 则相反，如果条件判断结果为 true 就返回第二个操作数的值，如果为 false 就返回第一个操作数的值。

|| 和 && 返回它们其中一个操作数的值，而非条件判断的结果

## 55.Symbol 值的强制类型转换？

ES6 允许从符号到字符串的显式强制类型转换，然而隐式强制类型转换会产生错误。

Symbol 值不能够被强制类型转换为数字（显式和隐式都会产生错误），但可以被强制类型转换为布尔值（显式和隐式结果都是 true ）。

```javascript
var s1 = Symbol('cool')
String(s1) // "Symbol(cool)"
var s2 = Symbol('not cool')
s2 + '' // TypeError
```

## 56. == 操作符的强制类型转换规则？

1. 字符串和数字之间的相等比较，将字符串转换为数字之后再进行比较。

2. 其他类型和布尔类型之间的相等比较，先将布尔值转换为数字后，再应用其他规则进行比较。

3. null 和 undefined 之间的相等比较，结果为真。其他值和它们进行比较都返回假值。

4. 对象和非对象之间的相等比较，对象先调用 ToPrimitive 抽象操作后，再进行比较。

5. 如果一个操作值为 NaN ，则相等比较返回 false（ NaN 本身也不等于 NaN ）。

6. 如果两个操作值都是对象，则比较它们是不是指向同一个对象。如果两个操作数都指向同一个对象，则相等操作符返回
   true，否则，返回 false。

详细资料可以参考： 《JavaScript 字符串间的比较》

## 57. 如何将字符串转化为数字，例如 '12.3b'?

1. 使用 Number() 方法，前提是所包含的字符串不包含不合法字符。

2. 使用 parseInt() 方法，parseInt() 函数可解析一个字符串，并返回一个整数。还可以设置要解析的数字的基数。
   当基数的值为 0，或没有设置该参数时，parseInt() 会根据 string 来判断数字的基数。

3. 使用 parseFloat() 方法，该函数解析一个字符串参数并返回一个浮点数。

4. 使用 + 操作符的隐式转换。

## 58. 如何判断一个对象是否属于某个类？

1. 是使用 instanceof 运算符来判断构造函数的 prototype 属性是否出现在对象的原型链中的任何位置。
2. 可以通过对象的 constructor 属性来判断，对象的 constructor 属性指向该对象的构造函数，但是这种方式不是很安全，因为 constructor 属性可以被改写。
3. 如果需要判断的是某个内置的引用类型的话，可以使用 Object.prototype.toString() 方法来打印对象的
   [[Class]] 属性来进行判断。

```javascript
function C() {}
function D() {}
var o = new C()

o instanceof C // true，因为 Object.getPrototypeOf(o) === C.prototype
o instanceof D // false，因为 D.prototype 不在 o 的原型链上
o.constructor == 'function C(){}' // true
```

## 59. 同步和异步的区别？

同步指的是当一个进程在执行某个请求的时候，如果这个请求需要等待一段时间才能返回，那么这个进程会一直等待下去，直到消息返回为止再继续向下执行。

异步指的是当一个进程在执行某个请求的时候，如果这个请求需要等待一段时间才能返回，这个时候进程会继续往下执行，不会阻塞等待消息的返回，当消息返回时系统再通知进程进行处理。

## 60. 原生对象和宿主对象

原生对象是 ECMAScript 规定的对象，所有内置对象都是原生对象，比如 Array、Date、RegExp 等；

宿主对象是宿主环境比如浏览器规定的对象，用于完善是 ECMAScript 的执行环境，比如 Document、Location、Navigator 等。

## 61. js 的几种模块规范？

js 中现在比较成熟的有四种模块加载方案。

1. CommonJS 方案，它通过 require 来引入模块，通过 module.exports 定义模块的输出接口。这种模块加载方案是服务器端的解决方案，它是以同步的方式来引入模块的，因为在服务端文件都存储在本地磁盘，所以读取非常快，所以以同步的方式加载没有问题。但如果是在浏览器端，由于模块的加载是使用网络请求，因此使用异步加载的方式更加合适。

2. AMD 方案，这种方案采用异步加载的方式来加载模块，模块的加载不影响后面语句的执行，所有依赖这个模块的语句都定义在一个回调函数里，等到加载完成后再执行回调函数。require.js 实现了 AMD 规范。

3. CMD 方案，这种方案和 AMD 方案都是为了解决异步模块加载的问题，sea.js 实现了 CMD 规范。它和 require.js 的区别在于模块定义时对依赖的处理不同和对依赖模块的执行时机的处理不同。

4. ES6 提出的方案，使用 import 和 export 的形式来导入导出模块。这种方案和上面三种方案都不同。

## 62. ES6 模块与 CommonJS 模块、AMD、CMD 的差异。

1. CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。CommonJS 模块输出的是值的拷贝，也就是说，一旦输出一个值，模块内部的变化就影响不到这个值。ES6 模块的运行机制与 CommonJS 不一样。JS 引擎对脚本静态分析的时候，遇到模块加载命令 import，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。

2. CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。CommonJS 模块就是对象，即在输入时是先加载整个模块，生成一个对象，然后再从这个对象上面读取方法，这种加载称为“运行时加载”。而 ES6 模块不是对象，它的对外接口只是 一种静态定义，在代码静态解析阶段就会生成。

CMD 是另一种 js 模块化方案，它与 AMD 很类似，不同点在于：AMD 推崇依赖前置、提前执行，CMD 推崇依赖就近、延迟执行。此规范其实是在 sea.js 推广过程中产生的。

```javascript
/** AMD写法 **/

define(['a', 'b', 'c', 'd', 'e', 'f'], function (a, b, c, d, e, f) {
  // 等于在最前面声明并初始化了要用到的所有模块
  a.doSomething()
  if (false) {
    // 即便没用到某个模块 b，但 b 还是提前执行了
    b.doSomething()
  }
})
/** CMD写法 **/

define(function (require, exports, module) {
  var a = require('./a') //在需要时申明
  a.doSomething()
  if (false) {
    var b = require('./b')
    b.doSomething()
  }
})
```

## 63. DOM 操作——怎样添加、移除、移动、复制、创建和查找节点？

1. 创建新节点

- createDocumentFragment(node)
- createElement(node)
- createTextNode(text)

2. 添加、移除、替换、插入

- appendChild(node)
- removeChild(node)
- replaceChild(new,old)
- insertBefore(new,old)

3. 查找

- getElementById()
- getElementsByName()
- getElementsByTagName()
- getElementsByClassName()
- querySelector()
- querySelectorAll()

4. 属性操作

- getAttribute(key)
- setAttribute(key,value)
- hasAttribute(key)
- removeAttribute(key)

64. 数组和对象有哪些原生方法，列举一下？

- 数组和字符串的转换方法：toString()、toLocalString()、join() 其中 join() 方法可以指定转换为字符串时的分隔符。

- 数组尾部操作的方法 pop() 和 push()，push 方法可以传入多个参数。

- 数组首部操作的方法 shift() 和 unshift()

- 重排序的方法 reverse() 和 sort()，sort() 方法可以传入一个函数来进行比较，传入前后两个值，如果返回值为正数，
  则交换两个参数的位置。

- 数组连接的方法 concat() ，返回的是拼接好的数组，不影响原数组。

- 数组截取办法 slice()，用于截取数组中的一部分返回，不影响原数组。

- 数组插入方法 splice()，影响原数组

- 查找特定项的索引的方法，indexOf() 和 lastIndexOf()

- 迭代方法 every()、some()、filter()、map() 和 forEach() 方法

- 数组归并方法 reduce() 和 reduceRight() 方法
