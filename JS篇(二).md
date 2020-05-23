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

65. requireJS 的核心原理是什么？（如何动态加载的？如何避免多次加载的？如何 缓存的？）

require.js 的核心原理是通过动态创建 script 脚本来异步引入模块，然后对每个脚本的 load 事件进行监听，如果每个脚本都加载完成了，再调用回调函数。

66. 如何判断当前脚本运行在浏览器还是 node 环境中？

```javascript
this === window ? 'browser' : 'node'
```

通过判断 Global 对象是否为 window，如果不为 window，当前脚本没有运行在浏览器中。

67. 移动端的点击事件的有延迟，时间是多久，为什么会有？ 怎么解决这个延时？

移动端点击有 300ms 的延迟是因为移动端会有双击缩放的这个操作，因此浏览器在 click 之后要等待 300ms，看用户有没有下一次点击，来判断这次操作是不是双击。

1. 通过 meta 标签禁用网页的缩放。
2. 通过 meta 标签将网页的 viewport 设置为 ideal viewport。
3. 调用一些 js 库，比如 FastClick

click 延时问题还可能引起点击穿透的问题，就是如果我们在一个元素上注册了 touchStart 的监听事件，这个事件会将这个元素隐藏掉，我们发现当这个元素隐藏后，触发了这个元素下的一个元素的点击事件，这就是点击穿透。

68. 什么是 Polyfill ？

Polyfill 指的是用于实现浏览器并不支持的原生 API 的代码。

比如说 querySelectorAll 是很多现代浏览器都支持的原生 Web API，但是有些古老的浏览器并不支持，那么假设有人写了一段代码来实现这个功能使这些浏览器也支持了这个功能，那么这就可以成为一个 Polyfill。

一个 shim 是一个库，有自己的 API，而不是单纯实现原生不支持的 API。

69. Object.is() 与原来的比较操作符 “===”、“==” 的区别？

使用双等号进行相等判断时，如果两边的类型不一致，则会进行强制类型转化后再进行比较。

使用三等号进行相等判断时，如果两边的类型不一致时，不会做强制类型准换，直接返回 false。

使用 Object.is 来进行相等判断时，一般情况下和三等号的判断相同，它处理了一些特殊的情况，比如 -0 和 +0 不再相等，两个 NaN 认定为是相等的。

70. toPrecision 和 toFixed 和 Math.round 的区别？

- toPrecision 用于处理精度，精度是从左至右第一个不为 0 的数开始数起。
- toFixed 是对小数点后指定位数取整，从小数点开始数起。可把 Number 四舍五入为指定小数位数的数字。( toFixed()生成的是字符串而不是数字)
- Math.round 是将一个数字四舍五入到一个整数

```javascript
var num = new Number(13.3714)
var a = num.toPrecision() // 13.3714
var b = num.toPrecision(2) // 13
var c = num.toPrecision(3) // 13.4
var d = num.toPrecision(10) // 13.37140000

var num = 5.56789
var n = num.toFixed(2) // 5.57

Math.round(2.5) // 3
```

71. js 拖拽功能的实现

- 相关知识点：

```
首先是三个事件，分别是 mousedown，mousemove，mouseup
当鼠标点击按下的时候，需要一个 tag 标识此时已经按下，可以执行 mousemove 里面的具体方法。
clientX，clientY 标识的是鼠标的坐标，分别标识横坐标和纵坐标，并且我们用 offsetX 和 offsetY 来表示
元素的元素的初始坐标，移动的举例应该是：
鼠标移动时候的坐标-鼠标按下去时候的坐标。
也就是说定位信息为：
鼠标移动时候的坐标-鼠标按下去时候的坐标+元素初始情况下的 offetLeft.
```

- 回答：

```
一个元素的拖拽过程，我们可以分为三个步骤，第一步是鼠标按下目标元素，第二步是鼠标保持按下的状态移动鼠标，第三步是鼠标抬起，拖拽过程结束

这三步分别对应了三个事件，mousedown 事件，mousemove 事件和 mouseup 事件。只有在鼠标按下的状态移动鼠标我们才会执行拖拽事件，因此我们需要在 mousedown 事件中设置一个状态来标识鼠标已经按下，然后在 mouseup 事件中再取消这个状态。在 mousedown
事件中我们首先应该判断，目标元素是否为拖拽元素，如果是拖拽元素，我们就设置状态并且保存这个时候鼠标的位置。然后在 mousemove 事件中，我们通过判断鼠标现在的位置和 以前位置的相对移动，来确定拖拽元素在移动中的坐标。
最后 mouseup 事件触发后，清除状态，结束拖拽事件。
```

72. 观察者模式和发布订阅模式有什么不同？

发布订阅模式其实属于广义上的观察者模式

在观察者模式中，观察者需要直接订阅目标事件。在目标发出内容改变的事件后，直接接收事件并作出响应。

而在发布订阅模式中，发布者和订阅者之间多了一个调度中心。调度中心一方面从发布者接收事件，另一方面向订阅者发布事件，订阅者需要在调度中心中订阅事件。通过调度中心实现了发布者和订阅者关系的解耦。使用发布订阅者模式更利于我们代码的可维护性。
详细资料可以参考： 《观察者模式和发布订阅模式有什么不同？》

73. 开发中常用的几种 Content-Type ？

1. application/x-www-form-urlencoded

浏览器的原生 form 表单，如果不设置 enctype 属性，那么最终就会以 application/x-www-form-urlencoded 方式提交数据。该种方式提交的数据放在 body 里面，数据按照 key1=val1&key2=val2 的方式进行编码，key 和 val 都进行了 URL 转码。

2. multipart/form-data

该种方式也是一个常见的 POST 提交方式，通常表单上传文件时使用该种方式。

3. application/json

告诉服务器消息主体是序列化后的 JSON 字符串。

4. text/xml

该种方式主要用来提交 XML 格式的数据。

74. instanceof 是否能正确判断类型？ instanceof 的实现原理是什么？

首先 typeof 能够正确的判断基本数据类型，但是除了 null, typeof null 输出的是对象。

但是对象来说，typeof 不能正确的判断其类型， typeof 一个函数可以输出 'function',而除此之外，输出的全是 object,这种情况下，我们无法准确的知道对象的类型。

instanceof 可以准确的判断复杂数据类型，但是不能正确判断基本数据类型。

instanceof 是通过原型链判断的，A instanceof B, 在 A 的原型链中层层查找，是否有原型等于 B.prototype，如果一直找到 A 的原型链的顶端(null;即 `Object.proptotype.__proto__`),仍然不等于 B.prototype，那么返回 false，否则返回 true.

instanceof 的实现代码：

```javascript
// L instanceof R
function instance_of(L, R) {
  // L 表示左表达式，R 表示右表达式
  var O = R.prototype // 取 R 的显式原型
  L = L.__proto__ // 取 L 的隐式原型
  while (true) {
    if (L === null)
      // 已经找到顶层
      return false
    if (O === L)
      // 当 O 严格等于 L 时，返回 true
      return true
    L = L.__proto__ // 继续向上一层原型链查找
  }
}
```

75. for of , for in 和 forEach,map 的区别。

- for...of 循环：具有 iterator 接口，就可以用 for...of 循环遍历它的成员(属性值)。for...of 循环可以使用的范围包括数组、Set 和 Map 结构、某些类似数组的对象、Generator 对象，以及字符串。for...of 循环调用遍历器接口，数组的遍历器接口只返回具有数字索引的属性。对于普通的对象，for...of 结构不能直接使用，会报错，必须部署了 Iterator 接口后才能使用。可以中断循环。

- for...in 循环：遍历对象自身的和继承的可枚举的属性, 不能直接获取属性值。可以中断循环。

- forEach: 只能遍历数组，不能中断，没有返回值(或认为返回值是 undefined)。

- map: 只能遍历数组，不能中断，返回值是修改后的数组。

76. 解释 JavaScript 中变量声明提升？什么是暂时性死区？

变量提升就是变量在声明之前就可以使用，值为 undefined。

-JavaScript 变量声明提升：

1. 在 JavaScript 中，函数声明与变量声明经常被 JavaScript 引擎隐式地提升到当前作用域的顶部
2. 声明语句中的赋值部分并不会被提升，只有名称被提升
3. 函数声明的优先级高于变量，如果变量名跟函数名相同且未赋值，则函数声明会覆盖变量声明
4. 如果函数有多个同名参数，那么最后一个参数（即使没有定义）会覆盖前面的同名参数

- 暂时性死区

在代码块内，使用 let/const 命令声明变量之前，该变量都是不可用的(会抛出错误)。这在语法上，称为“暂时性死区”。暂时性死区也意味着 typeof 不再是一个百分百安全的操作。

```javascript
typeof x // ReferenceError(暂时性死区，抛错)
let x

typeof y // 值是undefined,不会报错
```

暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。

77. 介绍 JavaScript 的原型，原型链？有什么特点？

- 原型：

1. JavaScript 的所有对象中都包含了一个 [proto] 内部属性，这个属性所对应的就是该对象的原型
2. JavaScript 的函数对象，除了原型 [proto] 之外，还预置了 prototype 属性
3. 当函数对象作为构造函数创建实例时，该 prototype 属性值将被作为实例对象的原型 [proto]。

- 原型链：

1. 当一个对象调用的属性/方法自身不存在时，就会去自己 [proto] 关联的前辈 prototype 对象上去找
2. 如果没找到，就会去该 prototype 原型 [proto] 关联的前辈 prototype 去找。依次类推，直到找到属性/方法或 undefined 为止。从而形成了所谓的“原型链”

- 原型特点：
  JavaScript 对象是通过引用来传递的，当修改原型时，与之相关的对象也会继承这一改变
