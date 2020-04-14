## 1.如何用原生js给一个按钮绑定两个onclick事件？

法1：
```javascript
btn.addEventListener("click",hello1);
btn.addEventListener("click",hello2);
```
法2：
```javascript
<button onclick="hello3();hello4()">绑定双事件</button>
```
## 2.请描述一下 cookies sessionStorage和localstorage区别

相同点：都存储在客户端

不同点：
1. 存储大小
- cookie数据大小不能超过4k。
- sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。
2. 有效时间
- localStorage    存储持久数据，浏览器关闭后数据不丢失除非主动删除数据；
- sessionStorage  数据在当前浏览器窗口关闭后自动删除。
- cookie    设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭

3. 数据与服务器之间的交互方式
- cookie的数据会自动的传递到服务器（始终在同源的http请求中携带），服务器端也可以写cookie到客户端
- sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。

## 3.document.write和innerHTML的区别
1. document.write是直接写入到页面的内容流，如果在写之前没有调用document.open, 浏览器会自动调用open。每次写完关闭之后重新调用该函数，会导致页面被重写。

2. innerHTML则是DOM页面元素的一个属性，代表该元素的html内容。你可以精确到某一个具体的元素来进行更改。如果想修改document的内容，则需要修改document.documentElement.innerElement。

3. document.write会重绘整个页面，而innerHTML是可以重绘页面的某一部分

## 4. js有几种数据类型，其中基本数据类型有哪些
- 五种基本类型: Undefined、Null、Boolean、Number和String。
- 复杂的数据类型————Object，Object本质上是由一组无序的名值对组成的。
- Object、Array和Function则属于引用类型

## 5.undefined 和 null 区别
- null： Null类型，代表“空值”，代表一个空对象指针，使用typeof运算得到 “object”，所以你可以认为它是一个特殊的对象值。

- undefined： Undefined类型，当一个声明了一个变量未初始化时，得到的就是undefined。

最后，总结就是： undefined是访问一个未初始化的变量时返回的值，而null是访问一个尚未存在的对象时所返回的值。因此，可以把undefined看作是空的变量，而null看作是空的对象。

> javaScript高级程序设计： 在使用var声明变量但未对其加以初始化时，这个变量的值就是undefined。null值则是表示空对象指针。

> javaScript权威指南： null 和 undefined 都表示“值的空缺”，你可以认为undefined是表示系统级的、出乎意料的或类似错误的值的空缺，而null是表示程序级的、正常的或在意料之中的值的空缺。

## 6.JS哪些操作会造成内存泄露
1. 意外的全局变量引起的内存泄露
```javascript
function leak(){  
  leak = "xxx"; // leak成为一个全局变量，不会被回收  
}
```
2. 闭包引起的内存泄露
3. 没有清理的DOM元素引用
4. 被遗忘的定时器或者回调
5. 子元素存在引起的内存泄露

## 7.js里面的异步操作有哪些？
1. 回调函数。 ajax典型的异步操作，利用XMLHttpRequest，回调函数获取服务器的数据传给前端。
2. 事件监听。 当监听事件发生时，先执行回调函数，再对监听事件进行改写
3. 观察者模式，也叫订阅发布模式。 多个观察者可以订阅同一个主题，主题对象改变时，主题对象就会通知这个观察者。
4. promise.
5. es7语法糖async/await
6. co库的generator函数

## 8. js做类型判断的方法有哪些？
1. typeof，可以判断原始数据类型：undefined、boolean、string、number、symbol，但是typeof null的类型判断为object。对于引用类型，会判断为function、object两种类型。
2. instanceof ,instanceof 检测的是原型，判断一个实例是否属于某种类型。
```javascript 
Object.prototype.toString.call(obj)
```
3. toString() 是 Object 的原型方法，调用该方法，默认返回当前对象的类型。

## 9.script 的位置是否会影响首屏显示时间？
浏览器解析 HTML 是自上而下的线性过程（会受到 js 执行的干扰。），script作为 HTML 的一部分同样遵循这个原则
DOM 构建的意思是，将文档中的所有 DOM 元素构建成一个树型结构。
注意，DOM 构建是自上而下进行构建的，会受到 js 执行的干扰。

因此，script 会延迟 DomContentLoad（DOMContentLoaded 事件在 html文档加载完毕，并且 html 所引用的内联 js、以及外链 js 的同步代码都执行完毕后触发。），只显示其上部分首屏内容，从而影响首屏显示的完成时间

参考：https://juejin.im/post/5b2a508ae51d4558de5bd5d1

## 10.关于JS事件冒泡与JS事件代理（事件委托）
1. 事件冒泡：

通俗易懂的来讲，就是当一个嵌套最深的子元素的事件被触发的时候（如onclick事件），该事件会从事件源（被点击的子元素）开始逐级向上传播，触发父级元素的点击事件。

2. 事件委托

这是一种让父元素上的事件监听器也影响子元素的技巧。事件委托，首先按字面的意思就能看出来，是将事件交由别人来执行，就是将子元素原本绑定的事件通过冒泡的形式交由父元素来执行。

- 使用事件代理的好处是可以提高性能
- 可以大量节省内存占用，减少事件注册，比如在table上代理所有td的click事件
- 可以实现当新增子对象时无需再次对其绑定

## 11.如何阻止事件冒泡和默认事件？
标准的DOM对象中可以使用事件对象的stopPropagation()方法来阻止事件冒泡，但在IE8以下中IE的事件对象通过设置事件对象的cancelBubble属性为true来阻止冒泡；

默认事件的话通过事件对象的preventDefault()方法来阻止，而IE通过设置事件对象的returnValue属性为false来阻止默认事件。

## 12.请解释JSONP的工作原理，以及它为什么不是真正的AJAX。
JSONP (JSON with Padding)是一个简单高效的跨域方式，HTML中的script标签可以加载并执行其他域的javascript，于是我们可以通过script标记来动态加载其他域的资源。

- 解释一：
例如我要从域A的页面pageA加载域B的数据，那么在域B的页面pageB中我以JavaScript的形式声明pageA需要的数据，然后在 pageA中用script标签把pageB加载进来，那么pageB中的脚本就会得以执行。JSONP在此基础上加入了回调函数，pageB加载完之后会执行pageA中定义的函数，所需要的数据会以参数的形式传递给该函数。JSONP易于实现，但是也会存在一些安全隐患，如果第三方的脚本随意地执行，那么它就可以篡改页面内容，截获敏感数据。但是在受信任的双方传递数据，JSONP是非常合适的选择。

- 解释二：
首先在客户端注册一个callback, 然后把callback的名字传给服务器。此时，服务器先生成 json 数据。然后以 javascript 语法的方式，生成一个function , function 名字就是传递上来的参数 jsonp.最后将 json 数据直接以入参的方式，放置到 function 中，这样就生成了一段 js 语法的文档，返回给客户端。
客户端浏览器，解析script标签，并执行返回的javascript文档，此时数据作为参数，传入到了客户端预先定义好的 callback 函数里.(动态执行回调函数) .

其实说白了，就是客户端定义一个函数(如a），请求地址后服务器端返回的结果是调用a函数,需要的数据都放在了a函数的参数里面。

AJAX是不跨域的，而JSONP是一个是跨域的，还有就是二者接收参数形式不一样！

参考：https://segmentfault.com/a/1190000002438126
## 13.请解释一下JavaScript的同源策略。
概念：
同源策略是客户端脚本（尤其是Javascript）的重要的安全度量标准，其目的是防止某个文档或脚本从多个不同源装载。
这里的同源策略指的是：协议，域名，端口相同，同源策略是一种安全协议，指一段脚本只能读取来自同一来源的窗口和文档的属性。

为什么要有同源限制：
我们举例说明：比如一个黑客程序，他利用Iframe把真正的银行登录页面嵌到他的页面上，当你使用真实的用户名，密码登录时，他的页面就可以通过Javascript读取到你的表单中input中的内容，这样用户名，密码就轻松到手了。
## 14.$(document).ready()方法和window.onload有什么区别？
1. window.onload方法是在网页中所有的元素(包括元素的所有关联文件)完全加载到浏览器后才执行的。
2. $(document).ready() 方法可以在DOM载入就绪时就对其进行操纵，并调用执行绑定的函数。

## 15.创建对象的三种方法/javascript创建对象的几种方式？
第一种方式，字面量
```javascript
var o1 = {name: "o1"}
```
第二种方式，通过构造函数
```javascript
var o2 = new Object({name: "o2"})
var M = function(name){ this.name = name }
var o3 = new M("o3")
```
第三种方式，Object.create
```javascript
var  p = {name: "p"}
var o4 = Object.create(p)
```
新创建的对o4的原型就是p，同时o4也拥有了属性name
Object.create()方法创建一个新对象，使用现有的对象来提供新创建的对象的__proto__

## 16.script标签的defer、async的区别
defer是在HTML解析完之后才会执行，如果是多个，按照加载的顺序依次执行
async是在加载完成后立即执行，如果是多个，执行顺序和加载顺序无关

对于defer，我们可以认为是将外链的js放在了页面底部。js的加载不会阻塞页面的渲染和资源的加载。不过defer会按照原本的js的顺序执行，所以如果前后有依赖关系的js可以放心使用。
 
对于async，这个是html5中新增的属性，它的作用是能够异步的加载和执行脚本，不因为加载脚本而阻塞页面的加载。一旦加载到就会立刻执行在有async的情况下，js一旦下载好了就会执行，所以很有可能不是按照原本的顺序来执行的。如果js前后有依赖性，用async，就很有可能出错。

当浏览器碰到 script 脚本的时候：

``` <script src="script.js"></script> ```

没有 defer 或 async，浏览器会立即加载并执行指定的脚本，“立即”指的是在渲染该 script 标签之下的文档元素之前，也就是说不等待后续载入的文档元素，读到就加载并执行。

``` <script async src="script.js"></script> ```

有 async，加载和渲染后续文档元素的过程将和 script.js 的加载与执行并行进行（异步）。

``` <script defer src="myscript.js"></script> ```

有 defer，加载后续文档元素的过程将和 script.js 的加载并行进行（异步），但是 script.js 的执行要在所有元素解析完成之后，DOMContentLoaded 事件触发之前完成。
无论使用defer还是async属性，都需要首先将页面中的js文件进行整理，哪些文件之间有依赖性，哪些文件可以延迟加载等等，做好js代码的合并和拆分，然后再根据页面需要使用这两个属性。

## 17.script标签如何异步加载？
默认情况下，浏览器是同步加载 JavaScript 脚本，即渲染引擎遇到 script 标签就会停下来，等到执行完脚本，再继续向下渲染。如果是外部脚本，还必须加入脚本下载的时间。如果脚本体积很大，下载和执行的时间就会很长，因此造成浏览器堵塞，用户会感觉到浏览器“卡死”了，没有任何响应。
```javascript
<script src="path/to/myModule.js" defer></script>
<script src="path/to/myModule.js" async></script>
```
defer与async的区别是： 
defer是“渲染完再执行”，async是“下载完就执行”。
另外，如果有多个defer脚本，会按照它们在页面出现的顺序加载，而多个async脚本是不能保证加载顺序的。

## 18.介绍js有哪些内置对象？

全局的对象（ global objects ）或称标准内置对象，不要和 "全局对象（global object）" 混淆。这里说的全局的对象是说在
全局作用域里的对象。

标准内置对象的分类

（1）值属性，这些全局属性返回一个简单值，这些值没有自己的属性和方法。
     
    例如 Infinity、NaN、undefined、null 字面量

（2）函数属性，全局函数可以直接调用，不需要在调用时指定所属对象，执行结束后会将结果直接返回给调用者。

    例如 eval()、parseFloat()、parseInt() 等
 
（3）基本对象，基本对象是定义或使用其他对象的基础。基本对象包括一般对象、函数对象和错误对象。
     
    例如 Object、Function、Boolean、Object、Array、Number Symbol、Error 等

（4）数字和日期对象，用来表示数字、日期和执行数学计算的对象。

    例如 Number、Math、Date 

（5）字符串，用来表示和操作字符串的对象。
    
    例如 String、RegExp

（6）可索引的集合对象，这些对象表示按照索引值来排序的数据集合，包括数组和类型数组，以及类数组结构的对象。例如 Array

（7）使用键的集合对象，这些集合对象在存储数据时会使用到键，支持按照插入顺序来迭代元素。
     
    例如 Map、Set、WeakMap、WeakSet
 
（8）矢量集合，SIMD 矢量集合中的数据会被组织为一个数据序列。

    例如 SIMD 等

（9）结构化数据，这些对象用来表示和操作结构化的缓冲区数据，或使用 JSON 编码的数据。

    例如 JSON 等

（10）控制抽象对象

    例如 Promise、Generator 等

（11）反射

    例如 Reflect、Proxy

（12）国际化，为了支持多语言处理而加入 ECMAScript 的对象。

    例如 Intl、Intl.Collator 等

（13）WebAssembly

（14）其他

     例如 arguments

## 19.说几条写JavaScript的基本规范？

1. 不要在同一行声明多个变量。
2. 请使用 ===/!==来比较true/false或者数值
3. 使用对象字面量替代new Array这种形式,用{}和[]声明对象和数组
4. 不要使用全局函数。
5. Switch语句必须带有default分支
6. 函数不应该有时候有返回值，有时候没有返回值。
7. For循环，If语句必须使用大括号

## 20.javascript 代码中的"use strict";是什么意思 ? 使用它区别是什么？

use strict 出现在 JavaScript 代码的顶部或函数的顶部，可以帮助你写出更安全的 JavaScript 代码,这种模式使得 Javascript 在更严格的条件下运行,

1. JS编码更加规范化的模式,消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为。
   默认支持的糟糕特性都会被禁用，比如不能用with，也不能在意外的情况下给全局变量赋值;
   全局变量的显性声明,函数必须声明在顶层，不允许在非函数代码块内声明函数,arguments.callee也不允许使用；
2. 消除代码运行的一些不安全之处，保证代码运行的安全,限制函数中的arguments修改，严格模式下的eval函数的行为和非严格模式的也不相同;
3. 提高编译器效率，增加运行速度；
4. 为未来新版本的Javascript标准化做铺垫。

## 21.Javascript中，有一个函数，执行时对象查找时，永远不会去查找原型，这个函数是？

**hasOwnProperty**

javaScript中hasOwnProperty函数方法是返回一个布尔值，指出一个对象是否具有指定名称的属性。此方法无法检查该对象的原型链中是否具有该属性；该属性必须是对象本身的一个成员。
使用方法：
```
object.hasOwnProperty(proName)
其中参数object是必选项。一个对象的实例。
proName是必选项。一个属性名称的字符串值。
```
如果 object 具有指定名称的属性，那么JavaScript中hasOwnProperty函数方法返回 true，反之则返回 false。

## 22.讲一下let、var、const的区别

- var 没有块级作用域，支持变量提升。
- let 有块级作用域，不支持变量提升。不允许重复声明，暂存性死区。
- const 有块级作用域，不支持变量提升，不允许重复声明，暂存性死区。声明一个变量一旦声明就不能改变，改变报错。const保证的变量的内存地址不得改动。
- const定义时必须赋值，let不必。

## 23.谈谈如何冻结变量

如果想要将对象冻结的话，使用Object.freeze()方法。

Object.freeze() 方法可以冻结一个对象。一个被冻结的对象再也不能被修改；冻结了一个对象则不能向这个对象添加新的属性，不能删除已有属性，不能修改该对象已有属性的可枚举性、可配置性、可写性，以及不能修改已有属性的值。此外，冻结一个对象后该对象的原型也不能被修改。
```javascript
const obj = {
    prop: 42
};

Object.freeze(obj);

obj.prop = 33;
// Throws an error in strict mode

console.log(obj.prop);
// expected output: 42
```

## 24.谈谈js事件循环机制

JavaScript的执行机制简单来说就先执行同步代码，然后执行异步代码，而异步的代码里面又分为宏任务代码和微任务代码，先执行微任务，然后执行宏任务。首先会将所有JavaScript作为一个宏任务执行，遇到同步的代码就执行，然后开始分配任务，遇到宏任务就将它的回调分配到宏任务的队列里，遇到微任务的回调就分配到微任务的队列里，然后开始执行所有的微任务。执行微任务的过程还是遵循先同步然后分配异步任务的顺序，微任务执行完毕之后，一次Event-Loop的Tick就算完成了。接着挨个去执行分配好的宏任务，在每个宏任务里又先同步后分配异步任务，完成下一次Tick，循环往复直到所有的任务全部执行完成。
 
微任务包括：process.nextTick ，promise ，MutationObserver。
宏任务包括：script ， setTimeout ，setInterval ，setImmediate ，I/O ，UI rendering。

## 25.Number('123')和new Number('123')有什么区别？

Number('123')是一个转换函数，会尝试把参数转为整数类型；而new Number('123')则不同，这是一个构造函数，它的结果是实例化出来一个对象。
```javascript
typeof Number('123') // number
typeof new Number('123') // object
```

## 26.在类型转换中哪些值会被转为true？

除了undefined、null、false、NaN、''、0、-0以外的值都会被转为true，包括所有引用类型，即使是空的。

## 27. 什么是基本包装类型？

基本类型并不是对象，是不应该有各自方法的，为什么能调用各自的那些方法，是因为在后台对基本类型进行了包装。例如字符串、整数、布尔值，首先会使用各自的构造函数创建对应的实例，这样调用这些方法时就可以正常使用，不过在方法调用结束后，就会将实例给销毁掉，从而又是基本类型。
```javascript
let s1 = 'hello'
let s2 = s1.substring(2)
```
↓ 后台包装
```javascript
let s1 = new String('hello') // 包装
let s2 = s1.substring(2) // 可以调用方法
s1 = null // 销毁
```

## 28. toString()和valueOf的区别？

null和undefined没有以上两个方法。
 
toString：值类型时返回自身的字符串形式；当是引用类型时，无论是一维或多维数组，将他们拍平成一个字符串，里面的null和undefined转为空字符串''，对象转为[object Object]，函数的原样返回字符串形式。
 
valueOf无论是值类型还是引用类型，大部分情况下都是原样返回，当是Date类型时，返回时间戳。
 
在进行字符串强转的时候，toString会优先于valueOf；在进行数值运算时，valueOf会优先于toString。
 
当执行toString的变量是一个整数类型时，支持传参，表示需要转为多少进制的字符串。

## 29. 什么是垃圾回收机制？

在程序执行的过程中，解释器会为创建出来的变量分配内存来存储这些变量的实体，执行环境会负责管理代码执行过程中使用到的内存，而何时划出新的内存以及何时把占用的内存释放出来的这样一套内存自动管理机制就是垃圾回收机制。这种周期性的回收策略主要有两种。
 
标记清除：当变量进入执行环境时标·记为“进入环境”，当变量离开执行环境时则标记为“离开环境”，被标记为“进入环境”的变量是不能被回收的，因为它们正在被使用，而标记为“离开环境”的变量则可以被回收

引用计数：追踪记录每个值被引用的次数，当声明了一个变量并将一个引用类型赋给该变量时，这个变量的引用次数就是1。相反如果包含这个值引用的变量又取得了另外一个值，则这个值的引用次数减1。当为0时，这说明没有办法再访问这个值了，因此垃圾收集器下次运行时，就会释放该值占用的内存。
在现代浏览器中，Javascript 使用的方式是标记清除，所以我们无需担心循环引用的问题

## 30. 如何解决引用类型变量共享的问题？

可以对引用类型进行深拷贝解决，最简单暴力的深拷贝是JSON.parse(JSON.stringify(obj))，不过也会存在诸多问题，更加完善的深拷贝需要手写递归方法对不同参数分别处理

## 31. 在 JavaScript 中，两种不同的内置类型间的转换被称为强制转型。强制转型在 JavaScript 中有两种形式：显式和隐式。

这是一个显式强制转型的例子
```javascript
var a = "42";
var b = Number( a );
a;                // "42" -- 字符串
b;                // 42 -- 是个数字!
```
这是一个隐式强制转型的例子
```javascript
var a = "42";
var b = a * 1;    // "42" 隐式转型成 42 
a;                // "42"
b;                // 42 -- 是个数字!
```

## 32. JavaScript 中的作用域（scope）是指什么？

1. 作用域是指程序源代码中定义变量的区域，描述的就是查找变量的范围
2. 作用域规定了如何查找变量，也就是确定当前执行代码对变量的访问权限。
3. 在 JavaScript 中，每个函数都有自己的作用域，只有函数中的代码才能访问函数作用域内的变量。
4. 同一个作用域中的变量名必须是唯一的。一个作用域可以嵌套在另一个作用域内。如果一个作用域嵌套在另一个作用域内，最内部作用域内的代码可以访问另一个作用域的变量。

## 33. 解释什么是回调函数，并提供一个简单的例子。

回调函数是可以作为参数传递给另一个函数的函数，并在某些操作完成后执行。下面是一个简单的回调函数示例，这个函数在某些操作完成后打印消息到控制台。
```javascript
function modifyArray(arr, callback) {
  // 对 arr 做一些操作
  arr.push(100);
  // 执行传进来的 callback 函数
  callback();
}

var arr = [1, 2, 3, 4, 5];

modifyArray(arr, function() {
  console.log("array has been modified", arr);
});
```

## 34. 如何检查一个数字是否为整数？

检查一个数字是小数还是整数，可以使用一种非常简单的方法，就是将它对 1 进行取模，看看是否有余数。
```javascript
function isInt(num) {
  return typeof num === 'number' && num % 1 === 0; 
}
//如果是string类型字符串內容是数字，会被转为整数


console.log(isInt(4)); // true
console.log(isInt(12.2)); // false
console.log(isInt(0.3)); // false
```