# JavaScript 小测验

这是一个 JavaScript 相关语言知识的小测验。

## 数据类型

### 如何理解 JavaScript 是一门弱类型语言？

Answer：JavaScript 具有多种数据类型的动态语言，之所以说其是一门弱类型语言，是因为其变量是弱类型的，即变量并不会绑定固定的类型。

### JavaScript 具有哪些数据类型，分类状况如何？

Answer：JavaScript 的语言类型主要分为两类：原始类型和对象类型。原始类型包括：数字、字符串、布尔值、null、undefined 和 Symbol，对象类型只包括对象。

### `typeof` 操作符的作用是什么？其可能的返回值有哪些？

Answer：`typeof` 操作符的作用是返回操作数的类型，其支持两种语法：`typeof x` 和 `typeof(x)`。其可能的返回值可以参考如下的代码输出：

```javascript
typeof undefined // 'undefined'
typeof 0 // 'number'
typeof true // 'boolean'
typeof 'foo' // 'string'
typeof Symbol('id') // 'symbol'
typeof Math // 'object'
typeof null // 'object'
typeof alert // 'function'
```

其中，`typeof null` 的结果是 `object`，这是语言本身的问题；`typeof alert` 的结果是 `function`，其实 `alert` 属于对象，只是这里为了方便使用，如此规定。

Note：`typeof(x)` 这种语法的调用虽然是正确的，可是 `typeof` 本身并不是一个函数，而是一个操作符。

### 什么是类型转换？简单类型之间的转换情况如何？

Answer：经常的，一些操作符和方法需要会自动将值转换为正确的类型，这个过程就是类型转换。对原始类型而言，可以使用包装器进行转换。

ToString：可以通过 `String(value)` 实现，对于原始类型而言，转换一般都是很简单的。

ToNumber：可以通过 `Number(value)` 实现，其转换规则如下：

| 值 | 转换之后的值 |
| :-- | :-- |
| undefined | NaN |
| null | 0 |
| true/false | 1/0 |
| string | 移除字符串起始和末尾的空格，此时如果为空字符串返回 0，否则尝试解析为数字，失败则返回错误 |

ToBoolean：在逻辑操作中经常使用到，可以通过 `Boolean(value)` 实现，其转换规则如下：

| 值 | 转换之后的值 |
| :-- | :-- |
| 0, null, undefined, NaN, "" | false |
| 其他任何值 | true |

## 比较

### 字符串、数字、布尔值之间的比较方法是怎样的？请写出下面这段代码的输出结果。

Answer：比较的返回值总是布尔值，即 `true` 和 `false`。

- 对于两个字符串比较，根据字符串各位字符的代码值比较
- 布尔值比较，`true > false` 
- 不同类型的比较，会将操作数都转换成数字，然后比较大小


```javascript
let a = 0
console.log(Boolean(a)) // false
let b = '0'
console.log(Boolean(b)) // true
console.log(a == b) // true
```

### `null`、`undefined` 与其他类型数据的比较关系（`==`、`===`、`>`、`<`、`>=`、`<=`）如何？

Answer：可以通过如下的代码输出理解，需要注意的是 `null` 和 `undefined` 值在不严格比较下是相等的，其他情况下以及与其他类型进行比较都是不相等的。

```javascript
null == undefined // true
null === undefined // false
```

### 如何理解下面的这段代码的输出情况？

```javascript
console.log(null > 0) // false
console.log(null == 0) // false
console.log(null >= 0) // true
```

Answer：真正的原因在于 `==` 与 `>`、`>=` 不属于同一类型的运算符。在 `null` 的不严格比较中，其只与自身和 `undefined` 相等。而在 `>`、`>=` 比较中，`null` 会被转换为 数字 0，因此有了上述的输出。

## 浏览器模态框

### 浏览器模态框函数有哪些？各自的功能、参数、返回值如何？

Answer：浏览器模态框函数只能在浏览器环境中使用，显示情况因浏览器而异，用于用户交互，包括：`alert`、`prompt`、`confirm` 三个函数，各自的描述如下：

| 模态框函数 | 功能 | 参数 | 返回值 |
| ---- | ---- | ---- | ---- | ---- |
| `alert(message)` | 显示消息并终止脚本的执行，直到用户点击确认 | `message` 表示提示的信息，任何类型都将转换为字符串 | 返回值为 `undefined` |
| `prompt(title[, default])` | 显示模态框提示用户输入并终止脚本的执行，直到用户点击确认 | `title` 为模态框名称，`default` 为默认的返回值 | 返回值为用户输入的字符串，默认值 `default`，不提供 `default` 时返回 `undefined` |
| `confirm(question)` | 显示需要确认的消息并终止脚本的执行，直到用户点击确认 | `question` 为需要确认的消息 | 确认时返回 `true`，否则返回 `false` |

## 表达式和运算符

### `switch` 表达式中使用的判断是否是严格等于？

Answer：`switch` 表达式中对于值得判断使用的是严格等于，也就意味着 `switch` 只能比较简单类型。

### 逻辑运算符有哪几种？优先级情况如何？什么是逻辑运算符的短路现象？

Answer：逻辑运算符有 `&&`、`||` 和 `!`，优先级为 `! > && > ||`。逻辑运算符的短路现象存在于 `&&` 和 `||` 中，对于 `&&`（或 `||`）而言，只有左边有一个操作数判断为 `false`（或 `true`），那么整个表达式的值就已经确定了，这时将不会计算并判断右边的操作数。

### 运算符 `+` 的用法有哪些？

Answer：`+`运算符的用法如下：
- 将值转换为对应的数值，类似于 `Number(value)` 的效果
- 数值相加，例如：`1 + 1`
- 字符串拼接，例如：`'1' + 1`

## 对象

### `.` 和 `[]` 访问方式的区别如何？对象属性的键可能是哪些类型？



### 什么是计算属性？属性-值缩写在何种情况下使用？


### 对象方法简写与常规书写方法的区别在哪里？


### 如何检测对象上是否存在指定的属性？


### 如何遍历对象的属性？遍历得到的属性顺序是怎样决定的？


### 对象间的比较是怎样的？

Answer：对象间的 `==` 和 `===` 比较效果是一样的，只有当两个对象是引用同一个对象时，比较结果才为 `true`。

### 常对象是什么？如何理解其本质？
### 什么是深复制和浅复制？如何实现对象的深复制？
### 对象到原始类型的转换是如何进行的？如何修改默认的转换行为？
### 对象的 Symbol.toPrimitive、 `toString` 和 `valueOf` 方法有何区别和联系？
### 如何检查一个构造器是否使用 `new` 运算符调用？以及如何解决非构造器调用问题？请以下面的代码写出解决方案。

```javascript
function User(name) {
  this.name = name
}
```

在构造器内部检测 `new.target` 的值，如果为 `undefined`，那么就是非构造器调用。解决方案如下：

```javascript
function User(name) {
  if (!new.target) {
    return new User(name)
  }
  this.name = name
}
```
### 构造器调用返回的默认值是什么？返回值的修改规则是怎样的？
### 什么是对象包装器？其作用是什么？请写出下面这段代码的输出。

```javascript
let str = 'hello'
str.test = 5
console.log(str.test)
```

## 垃圾回收

### JavaScript 引擎如何进行垃圾回收？

可达性

### “标记和清理”算法是怎样工作的？
### JavaScript 引擎提供了哪些垃圾回收优化方案？

- 分代回收
- 增量回收
- 闲时回收

## Symbol 类型

### Symbol 类型代表什么？和其他类型相比，Symbol 类型的特殊之处有哪些？

特殊之处：
- 独一无二，每一个 Symbol 一定是唯一的
- 不会自动转换为字符串，然而其他类型的数据会在一些情况下自动转换为字符串
- 可以作为对象的隐藏属性，会被 `for..in` 循环所忽略，但是仍然可以通过 `Object.getOwnPropertySymbols(obj)` 和 `Reflect.ownKeys(obj)` 获取

### 什么是全局 Symbol？如何创建和访问全局 Symbol 以及获取全局 Symbol 名称？
### 系统内置的 Symbol 有哪些？各自的作用如何？

## 数字

### 数字的 `toString(base)` 方法中 `base` 的取值范围是什么？如何将 `123456` 转换为 36 进制字符串？

`base` 的取值必须能够转换为范围为 2-36 之间的整数。

将 `123456` 转换为 36 进制字符串有如下两种方法：
- 在数字上直接调用方法必须使用双 `.`，而不能只使用单 `.`，因为 JavaScript 会把第一个 `.` 认为是小数点，即 `123456..toString(36)`
- 使用括号将 `123456` 包围，之后开始调用，即 `(123456).toString(36)`

### 如何使用 `Math` 对象的内置方法对数字进行取整？各个方法之间的区别如何？

`Math.floor`、`Math.ceil`、`Math.round`、`Math.trunc`

### 写出下面的输出结果。

```javascript
console.log(1e400)
console.log(.1 + .2 == .3)
```

### 字符串到数字的严转换与软转换的方法有哪些？

### 数字方法 `toFixed(digit)` 中的参数 `digit` 取值范围如何？如何理解下面的代码结果？

```javascript
console.log(1.35.toFixed(1)) // 1.4
console.log(6.35.toFixed(1)) // 6.3
```

`digit` 的取值必须能够转换为范围为 0-20 之间整数，不一定是整数。

### 下面的方法 `randomInteger(min, max)` 的功能是随机返回 `min` 和 `max` 之间的一个整数，请说明其为什么是错误的，并给出正确答案。

```javascript
function randomInteger(min, max) {
  let rand = min + Math.random() * (max - min)
  return Math.round(rand)
}
```

## 字符串

### 如何理解写法 `if(~str.indexOf('substr'))`？
### 获取一个字符串的子串的方法有哪些？有着怎样的区别？

- `str.slice(start[, end])`：返回从 `start` 到 `end` 之间的部分（不包括 `end`），使用的最多
- `str.substring(start[, end])`：返回从 `start` 到 `end` 之间的部分（不包括 `end`），且 `start` 可以比 `end` 大
- `str.substr(start[, length])`：返回从 `start` 开始指定长度 `length` 的部分

## 数组

### 数组的 `splice` 和 `slice` 方法有何作用和区别？

### 数组的 `indexOf` 和 `findIndex` 方法有何作用和区别？

### 如何理解数组的 `sort` 方法的“怪异行为”？如何解决这种行为？

```javascript
let arr = [1, 2, 15]
arr.sort()
console.log(arr) // [1, 15, 2]
```

### 如何判断一个值是否是数组？
### 如何让一个对象支持迭代即 `for..of`？如何获取对象的键、值？

- 针对 `Map`、`Set` 对象，通过 `obj.keys()`、`obj.values(obj)`、`obj.entries()` 方法即可获取键、值**迭代器**
- 针对其他普通对象，包括数组等，通过 `Object.keys(obj)` 方法即可获取键、值**数组**

### 下面的解构为什么出错？请给出解决方案。

```
let title, width, height
{title, width, height} = {title: 'menu', width: 200, height: 100}
```

## 日期

### `Date()` 和 `new Date()` 调用的区别如何？

`Date()` 和 `new Date()` 调用都会返回当前时间，但是 `Date()` 以字符串的形式返回时间，`new Date()` 以日期对象的形式返回当前时间。

### 如何创建一个日期对象？

- `new Date()` 返回当前时间的对象
- `new Date(milliseconds)` 返回 1970.1.1 开始经过 `milliseconds` 毫秒之后的时间，这个毫秒数就是所谓的时间戳（可以通过日期对象的 `getTime()` 方法获取）
- `new Date(datestring)` 使用 `Date.parse` 算法将传入的 `datestring` 字符串解析为时间
- `new Date(year, month, date, hours, minutes, seconds, ms)` 使用指定的这些参数构建时间，其中 `year` 必须是 4 位数字；`month` 的取值为 0 到 11 之间的整数；`date` 为月的天，默认为 1；`hours/minutes/seconds/ms` 默认值为 0

### 比较下面两个方法的性能。

```javascript
function diffSubstract(date1, date2) {
  return date2 - date1
}
function diffGetTime(date1, date2) {
  return date2.getTime() - date1.getTime()
}
```

## JSON

### JSON 的本质是什么？JSON 支持序列化哪些类型？对于不支持的类型其是如何处理的？

### 如何处理 `JSON.stringify(obj)` 中可能会带来的嵌套问题？

- `JSON.stringify(value)` 会在 `toJSON` 方法存在，则自动调用之，可以修改或设置此方法使其返回正确的值
- `JSON.stringify(value[, replacer, space])` 中的 `replacer` 可以作为字段数组或方法传入，以对特定字段进行特定处理

### 如何处理 `JSON.parse(str)` 中的日期字符串，使其返回正确的对象？

`JSON.parse(str[, reviver])` 中可以传入 `reviver` 方法，对特定字段的初始值处理后返回需要的结果。

## 函数

### 什么是执行环境？

执行环境是一个内部的数据结构，包含一个函数执行的信息：当前所在位置、当前的变量、`this` 值和一些其他信息。

当一个方法调用其他方法时，会发生：

- 当前方法被停止
- 执行环境被存储在执行环境调用栈
- 调用函数
- 当函数调用结束时，将从执行环境栈中获取之前的执行环境，并重新从之前的函数停止的地方重新开始执行

## 异常

### `try..catch` 只能捕捉哪类错误？

Answer：`try..catch` 只能捕捉运行时错误，所以说，代码必须是可运行的，即必须是有效的 JavaScript 代码。比如，当代码本身的语法就是错误的时候，那么 `try..catch` 就是无效的。当然，`catch` 和 `finally` 都不是必须的，但是至少存在一个，也就是说 `try` 可以不捕获异常。

### `try..catch` 是同步的，还是异步的？

Answer：如果 `try` 代码块中的异步代码出现了异常，那么在 `catch` 代码块中是无法捕捉此异常的。要想捕捉异步代码中的异常，必须将捕捉异常的代码放在异步中。

### `try..catch..finally` 中如果在 finally 执行之前，显式地使用 `return` 退出，那么 `finally` 代码块还会执行吗？

Answer：`finally` 代码块中的代码仍会执行，因为 `finally` 代码块对 `try..catch` 中的任何退出都会起作用，但是代码块 `return` 之后的代码是不会运行的，这和预期是一样的。

### 如何在浏览器中全局捕捉异常？

Answer：
```javascript
window.onerror = function(message, url, line, col, error) {
  // ...
}
```

## 表单

### 如何获取或设置表单元素的值？

Answer：
- `input` 元素：`input.value`，针对复选框使用 `input.checked`
- `textarea` 元素：`textarea.value` 而不是 `textarea.innerHTML`
- `select` 元素：`select.options` 获取其 `option` 子元素的集合，`select.value` 获取选择的 `option` 的值，`select.selectedIndex` 获取选择的 `option` 的索引；通过设定指定 `option` 元素的 `option.selected` 的值为 `true`，通过设定 `select.value` 的值为指定 `option` 的值，通过设定 `select.selectedIndex` 的值为指定 `option` 的索引

### 是否可以通过在 `onblur` 方法中调用 `event.preventDefault()` 来防止失去焦点？`focus` 和 `blur` 事件是否支持冒泡和捕捉？

Answer：不能，因为 `onblur` 方法是在失去焦点后触发的。

`focus` 和 `blur` 事件不支持冒泡，但是支持捕捉，所以可以通过设定 `addEventListener` 的第三个参数为 `true` 来进行捕捉。另外，还可以通过绑定事件 `focusin` 和 `focusout`，因为这两个事件自带冒泡。

### 如何让一个元素允许获取焦点？该方法的使用规则是怎样的？

Answer：除 `<button>`、`<input>`、`<select>`、`<a>` 等元素外，`<div>`、`<span>`、`<table>` 等大多数元素是不允许获取焦点的，但是这可以通过 HTML 属性 `tabindex` 改变，其主要目的是支持 `Tab` 切换。

属性 `tabindex` 的值将决定其切换的顺序，允许的值范围为 -1 开始的整数，值较小的会先获取焦点。但是也有两个特殊值：

- `tabindex="0"`：这个元素将最后被切换到
- `tabindex="-1"`：这个元素切换时会被忽略

## 箭头函数

### 箭头函数和普通函数有哪些区别？

Answer：

- 箭头函数没有 `this`，即使是使用 `bind` 绑定也是无效的
- 箭头函数不能使用 `new` 运算符调用
- 箭头函数没有 `arguments`

