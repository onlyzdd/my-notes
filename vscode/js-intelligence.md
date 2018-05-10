# VSCode中的JavaScript Intelligence

借助于基于TypeScript的语言服务所提供的支持，VSCode为JavaScript、TypeScript的编写提供了功能强大且丰富的Intelligence，以帮助开发者更加方便快速地进行开发。

TypeScript在后台使用静态分析来更好地了解使用者的代码，主要的功能项包括：

- 基于类型推理的Intelligence
- 基于JSDoc的Intelligence
- 基于TypeScript声明文件的Intelligence
- 自动获取类型定义
- 使用引用指令
- 编写代码片段
- 设置语言模式和文件关联

## 基于类型推理的Intelligence

都知道，JavaScript是弱类型的语言，对于变量是没有显示的类型定义的，在VSCode中通过对上下文代码的分析，可以轻易推导类型，此过程叫“过程推理”。

```js
let nextNum = 10
nextNum // 数字，允许调用数字的方法
```

- 对于变量或属性，类型通常是用于对其进行初始化的值或最新的值类型
- 对于函数，可以从`return`语句中推断返回类型

## 基于JSDoc的Intelligence

在类型推理不支持的情况下，可以通过JSDoc（由`/**`开始，`*/`结束的注释块）显示注释来提供类型信息。若要为声明的对象提供特定类型，可以使用`@type`标记。

```js
/** @type {HTMLCanvasElement} */
let canvas // 接着，就允许你调用Canvas提供的方法了
```

同样的，对于函数的形参也是不支持类型推理的，但是可以通过`@param`标记来提供特定类型。

```js
/**
 * @returns {number}
 * @param a {number} - number1
 * @param b {number} - number2
 */
function sum(a, b) {
    return a + b
}
```

## 基于TypeScript声明文件的Intelligence

由于JavaScript和TypeScript都基于TypeScript提供的语言服务，可以在自定义的`.d.ts`文件中声明类型来提供JavaScript Intelligence。

```js
// my.d.ts
declare class Person {
    constructor(address: string, age: number);
    address: string;
    age: number;
    getAge(): number;
}
// index.js
/**
 * @param {Person} p - person
 */
function showAge(p) {
    console.log(p.getAge())
}
```

## 自动获取类型定义

目前，已经有很多的库的接口以`.d.ts`文件描述，此类库通常位于[DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped)。
默认情况下，语言服务将自动获取使用的库对应的描述文件，除此以外，也可以通过`jsconfig.json`（针对JS文件）、`tsconfig.json`（针对TS文件）进行配置，关于文件的配置请参考[VSCode中的jsconfig.json](https://code.visualstudio.com/docs/languages/jsconfig)。

不仅仅是常用的库都被包含进去了，另外还有一些比较特殊的描述文件，比如`chrome`，这是在Google Chrome浏览器中独有的对象，如果需要编写Chrome插件，这个对象非常重要，如果没有代码提示，那么写起来一定不会很舒服。

## 使用引用指令

通过引用指令，可以将当前的脚本与其他的脚本进行关联，通常就可以允许你在当前脚本文件中使用其他脚本中可用的变量，而不需要导入，不过这种声明必须位于文档的顶部。

```js
/// <reference path="./module.js" />
// 接着，你就可以使用module.js文件中提供的变量了
```

## 编写代码片段

代码片段（Snippet）是帮助快速插入重复代码的模板，VSCode中基于[TextMate snippet syntax](https://manual.macromates.com/en/snippets)提供了强大的代码片段功能，除了内置了一些代码片段之外，VSCode还允许你自己[创建代码片段](https://code.visualstudio.com/docs/editor/userdefinedsnippets)。

![AJAX Snippet](./js-intelligence/ajax-snippet.gif)

## 设置语言模式和文件关联

对于一些特别的文件，它们有着比较特别的文件扩展名，但内容上很大和HTML、CSS、JS等有着非常相似的语法结构。对于单个文件，可以修改文件的语言模式，对于同类文件，则可以设置文件关联，以提供相类似的智能提示。

```json
// .vscode/setting.json
{
    "files.associations": {
        "*.wxml": "html",
        "*.wxss": "css"
    }
}
```

## 参考

- [JavaScript中的JSDoc支持](https://github.com/Microsoft/TypeScript/wiki/JsDoc-support-in-JavaScript)
- [TypeScript文档](https://www.typescriptlang.org/docs/home.html)
- [VSCode中的jsconfig.json](https://code.visualstudio.com/docs/languages/jsconfig)
- [JavaScript Intelligence](https://msdn.microsoft.com/en-us/library/bb385682.aspx)
