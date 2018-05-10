# 重新认识JSON

**JSON**（JavaScript Object Notation）是一种轻量级的数据交换格式，在利于人阅读和编写的同时，也利于机器的理解和生成。JSON基于ECMA-262第三版的一个子集，采用独立于语言的文本格式，使其成为理想的数据交换语言。

## JSON的结构

JSON有两种结构，分别是：

- “键/值对”：对象、字典、关联数组，以“{”开始，以“}”结束
- 有序列表：值有序的数组，以“[”开始，以“]”结束

这里的键只能是字符串，而值（value）可以是双引号括起来的字符串（string）、数值(number)、true、false、 null、对象（object）或者数组（array）。这些结构可以嵌套。

- 字符串：是由双引号包围的任意数量Unicode字符的集合，使用反斜线转义
- 数字：JSON中的数字只支持十进制
- 布尔值：true和false
- 空：null

## JavaScript中的JSON操作

### JSON.stringify()

`JSON.stringify()`方法将传入的对象转换为JSON字符串，并允许替换值和保留的空格数，当有循环引用时，会提示`TypeError`。

```js
JSON.stringify(value[, replacer][, space])
```

其参数作用如下：

- value：需要转换的JavaScript对象
- replacer：如果是方法，接收键、值，用于替换值，返回新值；如果是数组，则取数组的各个值作为键的属性对
- space：为保证输出字符串易读，而将空白格替换为字符串或一定数目的空格

> 说明

1. 在值进行转换时，默认会调用值得`toJSON()`方法
2. 布尔值、数字、字符串等对象都会被转换为对应的原始值
3. 值`undefined`、`Function`和`Symbol`都会被忽略掉，但当这些值出现在数组中时，都会被转换为`null`
4. 以`Symbol`作为键的属性都会被忽略掉，即使是在`replacer`方法中
5. `Date`对象转换时，会调用`date.toISOString()`返回字符串
6. 数字`Infinity`、`NaN`以及`null`都会被处理为`null`
7. 对于`Map`、`Set`、`WeakMap`、`WeakSet`，只有可枚举的属性才会被序列化

```js
function replacer(key, value) {
  // Filtering out properties
  if (typeof value === 'string') {
    return undefined;
  }
  return value;
}

var foo = {foundation: 'Mozilla', model: 'box', week: 45, transport: 'car', month: 7};
JSON.stringify(foo, replacer);
// '{"week":45,"month":7}'
```

### JSON.parse()

`JSON.parse()`方法将传入的字符串转换为对象，并允许确定转换的方式，当转换的不是有效JSON时，会提示`SyntaxError`。

```js
JSON.parse(text[, reviver])
```

其参数作用如下：

- text：需要转换为对象的JSON字符串
- reviver：如果是方法，将确定字符串转换为对象的方式

```js
JSON.parse('{"p": 5}', (key, value) =>
  typeof value === 'number'
    ? value * 2 // return value * 2 for numbers
    : value     // return everything else unchanged
);
// { p: 10 }
```

## JSON5

如你所知，JSON是有很严格的格式要求的，为了解决这些约束，并且让JSON对人友好，有人提出了JSON5，这是JSON的一个超集。

里面增加的功能包括：注释、十六进制的数字、单引号、多行等，事实上，我们也会经常看到JSON5的文件，比如VSCode用的配置文件就是使用这个规范的，但是NPM项目的`package.json`用的就是JSON了。

## JSON Schema

随着JSON的发展，越来越多的开发中会把JSON作为配置文件，然而就像XML似的，对JSON结构进行验证也就有很大的必要了。

对此，比较好的解决方案就是JSON Schema，其提供了一些常用的Schema，而且也提供了编写自己的Schema文件的方法。

另外，在VSCode的JSON文件中，通过添加`$schema`字段，并指向Schema文件所在位置，就能提供JSON文件的自动补全，这在开发一些应用时是很方便的。

## 参考

- [MDN JSON.stringify()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)
- [MDN JSON.parse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)
- [JSON5-JSON for Humans](https://github.com/json5/json5)
- [JSON Schema](http://schemastore.org/json/)
- [VSCode-JSON Schema and Setting](https://code.visualstudio.com/docs/languages/json#_json-schemas-settings)
