# Nodejs 中的模块引入

Nodejs 中有两个模块用来管理模块依赖：`require` 模块和 `module` 模块，而且这两个模块都在全局作用域可用。

可以将 `require` 理解为引入模块的命令，而 `module` 则是引入模块的管理者。

当 `require()` 方法的参数是一个文件路径时，Node 将会按照下面的步骤处理：

1. 解析：找到文件的绝对路径
2. 加载：决定文件内容的类型
3. 包装：赋予文件自身的作用域
4. 执行：VM 根据加载的代码执行
5. 缓存：当我们再次加载这个文件时，就不用经过上述步骤了

## 解析文件路径

首先需要介绍的是 `module` 对象，可以通过 Node 的 REPL 会话知道：

```bash
$ node
> module
Module {  id: '<repl>',
  exports: {},  parent: undefined,
  filename: null,  loaded: false,
  children: [],  paths:
   [ '/Users/test/Desktop/study/notes/repl/node_modules',     '/Users/test/Desktop/study/notes/node_modules',
     '/Users/test/Desktop/study/node_modules',     '/Users/test/Desktop/node_modules',
     '/Users/test/node_modules',     '/Users/node_modules',
     '/node_modules',
     '/Users/test/.node_modules',
     '/Users/test/.node_libraries',
     '/usr/local/lib/node' ] }
> .exit
```

每一个模块对象都会有一个 `id` 属性用来标识这个模块，这个 `id` 通常是模块入口文件的全路径，但是在 REPL 会话中其值就是简单的 `<repl>`。`module` 对象的属性 `paths` 是一个数组，其是 Node 在解析文件的绝对路径所依次尝试的目录，直到找到正确的路径，如果无法解析此路径，则会抛出 `cannot find module error` 的错误。

### 引用一个文件夹

模块可以不是文件，其也可以是一个文件夹。当使用 `require` 引入一个文件夹时，其默认会使用文件夹下的 `index.js` 文件，当然这个可以通过该文件夹下的 `package.json` 文件中定义的 `main` 字段定义。

### require.resolve

如果想要解析一个模块，但是不执行它，可以使用 `require.resolve` 方法。其行为与 `require` 方法一直，只是不加载文件。其返回值为模块入口的路径字符串，当其解析失败时，会抛出错误。

`require.resolve` 方法可以用来检查某个模块是否可用，即是否被安装。

### 相对路径和绝对路径

除了从 `node_modules` 中解析模块以外，还可以通过相对路径（`./` 和 `../`）以及绝对路径（`/`）引入模块，此时模块解析会依照路径解析。

### 模块之间的层级关系

模块之间是有层次关系的，认为通过 `require` 引入的模块是当前模块的子模块。

## module 对象

### loaded 属性

loaded 属性标识这个模块是否之前被加载过，如果没被加载过或加载未完成，这个属性的值就是 `false`，如果被成功加载，就会将这个属性设置为 `true`。例如：

```javascript
setTimeout(() => {
  console.log(module) // 此时加载已完成，属性 loaded 的值已变为 true
})
```

```javascript
console.log(module) // 还未加载完成，属性 loaded 的值为 false
```

这也说明了，Node 中的模块加载是同步的。

### exports 属性

exports 属性规定了模块所导出的内容，默认情况下，`exports` 是对 `module.exports` 的引用，所以你应该修改 `exports` 的属性，但不能修改其引用。此外由于 Node 中的模块加载是异步的，所以在异步中声明 exports 都是无效的。即：

```javascript
fs.readFile('/etc/password', (err, data) => {
  if (err) throw err
  exports.data = data // this will not work
})
```

## JSON 和 C/C++ 插件

引入的模块并不要求一定为 JS 文件，对于 JSON 文件和 C/C++ 文件而是支持的，文件后缀分别为 `.json` 和 `.node`。

可以通过 `require.extensions` 获取 `require` 支持引入的文件类型，目前其返回值应该是：

```bash
$ node
> require.extensions
{ '.js': [Function], '.json': [Function], '.node': [Function] }
```

## 模块分离

与浏览器中不同的是，脚本的作用都是全局的，但是在 Node 中，Node 会提供相关的包装方法，将每一个模块包装起来，使内部逻辑不会应用于整体。可以通过 `require('module').wrapper`知道：

```javascript
$ node
> require('module').wrapper
[ '(function (exports, require, module, __filename, __dirname) { ',
  '\n});' ]
```

当你的模块第一行出错的时候，也可以看到上面这个方法。此外，你还可以执行下面的代码以查看 Node 对待模块的真正处理方式。

```javascript
console.log(arguments) // 将会打印出上面方法的各个参数
```







