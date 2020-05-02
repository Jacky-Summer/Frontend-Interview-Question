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
