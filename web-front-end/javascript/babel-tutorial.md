# Babel

Babel 是一个 JavaScript 编译器，它将允许你在代码中使用下一代 JavaScript。

## ES2015 和最新的语法

通过语法转换器，Babel 提供了最新版本 JavaScript 语言特性的支持，而不用等待浏览器支持。为此，需要引入包含语法转换器的预设集。

以 `env` 预设集为例，可以通过以下命令安装：

```
$ npm i babel-preset-env
```

其提供的语法包括：Arrow functions、Async functions、Async generator functions、Block scoping、Block scoped functions、Classes、Class properties、Computed property names、Constants、Decorators、Default parameters、Destrructuring、Do expressions、Exponentiation operator、For-of、Function bind、Generators、Modules、Module export extensions、New literals、Object rest/spread、Property method assignment、Property name shorthand、Rest parameters、Spread、Sticky regex、Template literals、Trailing function commas、Type annotations、Unicode regex。

## 填充

因为 Babel 的核心只转换语法（比如箭头函数），这时你需要使用 `babel-polyfill` 来支持一些全局变量以及一些原生方法等。

`babel-polyfill` 提供了 ArrayBuffer、Array.from、Array.of、Array#copyWithin、Array#fill、Array#find、Array#findIndex、Function#name、Map、Math.acosh、Math.hypot、Math.imul、Number.isNaN、Number.isInteger、Object.assign、Object.getOwnPropertyDescriptors、Object.is、Object.entries、Object.values、Object.setPrototypeOf、Promise、Reflect、RegExp#flags、Set、String#codePointAt、String#endsWith、String.fromCodePoint、String#includes、String.raw、String#repeat、String#startsWith、String#padStart、String#padEnd、Symbol、WeakMap、WeakSet 等填充，可以通过下面的命令安装。

```
$ npm i babel-polyfill
```

## JSX 和 Flow

通过 React 预设集 `babel-preset-react`，Babel 也可以支持转换 JSX 语法。

## 案例

首先安装 Babel 和一个预设集和填充：

```
$ npm i babel-cli babel-preset-env babel-polyfill
```

接着创建一个 `.babel` 文件（也可以使用 `package.json`），并添加:

```json
{
  "preset": ["env"]
}
```
