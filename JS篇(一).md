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