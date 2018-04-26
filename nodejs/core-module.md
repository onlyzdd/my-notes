# Node.js 核心模块

核心（原生）模块是指那些随 Node.js 安装而安装的模块，这些模块在 Node.js 源代码编译时被编译成二进制执行文件。相比文件模块，核心（原生）模块的加载速度更快。核心（原生）模块提供了JavaScript语言之外处理能力，如：网络处理、文件及流处理、二进制处理、系统与进程等。

## EventEmitter - 事件模块

大部分的 Node.js 核心 API 被实现为异步事件驱动架构，这些对象（发射器）会周期性地发射事件名，并会触发监听函数（监听器）的调用。

Node.js 中许多对象都可以发送事件，如：`net.Server` 对象会在每次收到新连接时发送 `request` 事件；`fs.ReadStream` 对象会在打开文件时发送 `open` 事件；`stream.Readable` 对象会在每次读取数据时发送 `data` 事件。

所有这些可以发送事件的对象，都是一个 `EventEmitter` 类的实例。它们会暴露一个 `eventEmiiter.on()` 函数，在指定事件发送时调用监听函数。

当 `EventEmitter` 对象发送事件时，其上绑定的对应函数就会被同步调用。但是，监听函数所有的返回值都会被忽略。

通过继承 `EventEmitter` 类后，其实例就是一个事件发射器：

```javascript
const EventEmitter = require('events')
const util = require('util')

function MyEmitter() {
  EventEmitter.call(this)
}

util.inherits(MyEmitter, EventEmitter)

const myEmitter = new MyEmitter()

myEmitter.on('event', () => {
  console.log('An event occurred!')
})

myEmitter.emit('event')
```

上例中通过 `util.inherits()` 方法实现了 Node.js 式继承。同样，也可以使用 ES6 Class。

### 向监听器传递参数

`emitter.emit()` 发射事件时，可以向监听函数传递参数。其第一个参数为事件名，其后参数为要传递给事件监听器的参数：

```javascript
const EventEmitter = require('events')
class MyEmitter extends EventEmitter {}

const myEmitter = new MyEmitter()

myEmitter.on('event', (a, b) => {
  console.log(a, b)
})

myEmitter.emit('event', 'a', 'b')
```

### 多次与单次监听

通过 `emitter.on()` 注册的事件监听器会在每次触发指定时调用，而通过 `emitter.once()` 方法注册的事件监听器只会被调用一次。

### Class: EventEmitter

`EventEmitter` 类是在 `events` 模块中定义并导出的类，该类的实例就是一个事件发射器，Node.js 中所有具有事件发射功能的对象都是该类的一个实例。可以通过下面的代码获取该类：

```javascript
const EventEmitter = require('events').EventEmitter
```

所有的 `EventEmitter` 类实例都有 `newListener` 和 `removeListener` 两个事件，分别会在添加新的事件监听器和移除事件监听器时触发。

默认情况下，所有监听器可以添加 10 个事件监听器，这由 `EventEmitter.defaultMaxListeners` 决定，可以通过此属性全局修改可添加事件监听器数量。另外可以通过 `emitter.setMaxListeners(n)` 方法修改每个实例的监听器数量。

```javascript
emiter.setMaxListeners(emitter.getMaxListeners() + 1)
emitter.once('event', () => {
  emitter.setMaxListeners(Math.max(emitter.getMaxListeners() - 1, 0))
})
```

`EventEmitter` 实例中常用的方法有：

- `emitter.addEventListener(eventName, listener)`：添加事件监听器，`emitter.on(eventName, listener)` 的别名方法
- `emitter.getMaxListeners()`：最多可添加的监听器数量
- `emitter.listenerCount(eventName)`：某事件的监听器数量
- `emitter.listeners(eventName)`：返回事件的所有监听器函数
- `emitter.on(eventName, listener)`：添加事件监听器
- `emitter.once(eventName, listener)`：添加仅适用一次的事件监听器
- `emitter.removeAllListeners([eventName])`：移除所有事件监听器
- `emitter.removeListener(eventName, listener)`：移除指定的事件监听器
- `emitter.setMaxListeners(n)`：设置最大可添加监听器数量，高于全局配置

## 网络处理

### `net` - 实现 TCP/Socket 通讯

`net` 模块提供了异步网络封装，我们可以使用这个模块实现 TCP 服务器或客户端（TCP Socket）。该模块是对 TCP 协议的封装和实现，同时它还为上层应用提供了传输功能，如：`http` 模块就依赖该模块实现。

`net` 模块中有 `net.Server` 和 `net.Socket` 两个类，并通过这两个类对象提供 TCP 服务器及 TCP Socket 相关功能。

#### Class: net.Server - TCP 服务器

`net.Server` 类用于创建一个 TCP 或本地服务器，该类实例由 `net.createServer()` 方法创建并返回：

```javascript
const net = require('net')

let server = net.createServer((socket) => {
  socket.end('GoodBye!')
}).on('error', (err) => {
  throw err
})

server.listen(() => {
  let address = server.address()
  console.log('Opened server on %j', address)
})
```

`net.Server` 是一个事件发射器，它会发射 `close`、`connection`、`error`、`listening` 事件，除了在创建服务器时添加连接监听函数外，还可以通过 `connection` 事件添加，其包含一个参数 `socket`，其是 `net.Socket` 的实例。

#### Class: net.Socket - TCP Socket

TCP 是一种面向连接的协议，Socket 套接字是对 TCP 协议的具体封装。其在 Node.js 中被封装为 `net.Socket` 类，服务器与客户端之间的通讯都基于该类的实例实现。

`net.Socket` 对象是一个抽象的 TCP 或本地套接。它可以由用户在客户端创建（`connect()`）；或者由 TCP 服务器创建，并被传递至 `connection` 事件的回调函数参数中。

在客户端创建 `net.Socket` 实例时，可以使用构造函数 `new net.Socket([options])`，也可以使用工厂方法 `net.connect()` 或者 `net.createConnection()`。使用工厂方法创建时，除会返回套接字实例外，还会自动连接至指定的 TCP 服务器：

```javascript
const net = require('net')

const client = net.connect({
  port: 8124
}, () => {
  console.log('Connected to server!')
  client.write('world!\r\n')
})

client.on('data', data => {
  console.log(data.toString())
  client.end()
})

client.on('end', () => {
  console.log('Disconnected from server!')
})
```

`net.Socket` 实现了双向流接口，它还是一个 EventEmitter。Node.js TCP 客户端与服务器之间的通讯是基于流的操作，如：设置编码 `socket.setEncoding()` 本质上是设置可读流的编码，其内部会调用 `stream.setEncoding()` 方法；除了其自身添加的事件外，还有部分事件（如上例中使用的 `data`、`end` 事件）继承自 `stream` 模块。

### `tls` - TLS（SSL）

`tls` 模块使用 OpenSSL 来提供传输层安全（Transport Layer Security）和安全套接字层（Secure Socket Layer）。该模块使用加密过的刘通讯，可以使用这个模块构建一个安全的 TCP 服务器或是一个安全的 Socket 套接字。在使用 `tls` 模块之前，应该首先创建 TLS/SSL 公钥、私钥。

#### Class: SecurePair

`SecurePair` 表示“密钥对”类，该类对象由 `tls.createSecurePair()` 创建并返回。对象中包含两个流，一个用于读/写加密数据，一个用于读/写明文数据。

#### Class: tls.Server

`tls.Server` 是 `net.Server` 的子类并与其有相同的方法。不同于原始 TCP 请求，`tls.Server` 服务器会使用 TLS 或 SSL 加密连接。

该类实例由 `tls.createServer()` 创建并返回：

```javascript
const tls = require('tls')
const fs = require('fs')

const options = {
  key: fs.readFileSync('server-key.pem'),
  cert: fs.readFileSync('server-cert.pem'),
  requestCert: true,
  ca: [fs.readFileSync('client-cert.pem')]
}

let server = tls.createServer(options, socket => {
  console.log('Server connected', socket.authorized ? 'authorized' : 'unauthorized')
  socket.write('Welcome!\n')
  socket.setEncoding('utf8')
  socket.pipe(socket)
})

server.listen(8000, () => {
  console.log('Server bound!')
})
```

或使用 `.pfx` 文件创建连接；

```javascript
const tls = require('tls')
const fs = require('fs')

const options = {
  pfx: fs.readFileSync('client.pfx')
}

let socket = tls.connect(8000, options, () => {
  console.log('client connected', socket.authorized ? 'authorized' : 'unauthorized')
  process.stdin.pipe(socket)
  process.stdin.resume()
})
socket.setEncoding('utf8')
socket.on('data', (data) => {
  console.log(data)
})
socket.on('end', () => {
  server.close()
})
```

### `dgram` - UDP/Datagram

UDP/Datagram，用户数据协议。UDP 和 TCP 同样属于 OSI 七层中传输层的协议。不同的是，TCP 是面向连接的协议，其建立连接会经过三次握手，并会通过 `keepAlive` 包监听终端在线情况；而 UDP 是一种无连接的协议，虽然 UDP 在数据传递过程中会出现数据包丢失的情况，但其优势在于，极低的带宽点用量会大大降低执行时间，使传输速度得到了保证。

#### Class: dgram.Socket

`dgram.Socket` 是 `dgram` 模块中的唯一一个类，该类实现了 UDP 广播、数据发送/接收等功能。创建该类的实例使用 `dgram.createSocket()` 方法，不支持 `new` 关键字创建 `dgram.Socket` 实例。

```javascript
const dgram = require('dgram')

const server = dgram.createSocket('udp4')

server.on('error', err => {
  console.log('Server error: ' + err.stack)
  server.close()
})

server.on('message', (msg, rinfo) => {
  console.log(`Server got: ${msg} from ${rinfo.address}:${rinfo.port}`)
  server.send(`收到了： ${msg}`, 0, msg.length, rinfo.port, rinfo.address)
})

server.on('listening', () => {
  let address = server.address()
  console.log(`Server listening at ${address.address}:${address.port}`)
})

server.bind(41234)
```

如上，我们创建了一个 UDP 服务器，并在 `0.0.0.0:41234` 启动监听，但这个服务器并不是真正意义上的服务器，只是一个套接字端点。

`dgram.Socket` 是一个 EventEmitter，它会发送 `close`、`error`、`listening`、`message` 事件。在上例中，通过 `message` 添加事件监听实现了消息的接收，还可以在创建对象时添加消息监听。在添加的回调函数中，包含两个参数：

```javascript
function (msg, rinfo) {}
```

其中，`msg` 表示收到的消息，`rinfo` 表示远程（发送消息方）套接字端点的地址信息。

接下来，再创建一个 UDP 端点（客户端），实现两个端点之间的数据收发：

```javascript
const dgram = require('dgram')
const client = dgram.createSocket('udp4')
let message = new Buffer('time')

client.send(message, 0, message.length, 41234, 'localhost', (err, bytes) => {
  if (err) throw err
  console.log(bytes)
})

client.on('message', msg => {
  console.log(`收到了 UDP 服务端消息：${msg.toString()}`)
}
```

### `http`

HTTP 是 OSI 七层中应用层的协议，其底层传输依赖 TCP。在 Node.js 中，`http` 模块也依赖于 `net` 模块。

Node.js 提供的 HTTP 相关 API 非常底层，它只做流处理和消息解析。而解析消息时，只将其解析为消息头和消息体，但并不解析具体的消息头或消息体。

`http` 模块给我们留了足够大的操作空间，利用这个模块可以创建 HTTP 服务器、进行 HTTP 请求或响应的处理、创建 HTTP 客户端或创建 HTTP 代理。

#### Class: http.ClientRequest - 客户端请求

该对象（类实例）在 `http.request()` 或 `http.get()` 方法的内部创建并返回，表示一个正在处理的 HTTP 请求：

```javascript
const http = require('http')
const querystring = require('querystring')

let postData = querystring.stringify({ msg: 'Hello, world!' })

let options = {
  hostname: 'localhost',
  port: 8080,
  path: '/upload',
  method: 'POST',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded',
    'Content-Length': postData.length
  }
}

let req = http.request(options, res => {
  let data = ''
  res.setEncoding('utf8')
  res.on('data', chunk => {
    data += chunk
  })
  res.on('end', () => {
    console.log(data)
  })
})

req.on('error', e => {
  console.log(`Problem with request: ${e.message}`)
})

req.write(postData)
req.end()
```

在上面这个 HTTP 客户端请求对象中，我们通过 `req.write()` 方法向请求体中写入数据，并通过 `req.end()` 方法结束请求。在 `http.request()` 回调函数中接收服务器返回数据，服务器返回的数据会封装到一个 `http.IncomingMessage` 对象（即上面的 `res`）中。

请求对象是一个可写流，还是一个事件发射器对象。

对于上面的示例而言，还可以向基本流请求体中写入数据，并通过监听 `req` 对象上的 `response` 来接收或处理服务器返回数据。

#### Class: http.Agent - 代理

`http.Agent` 会把套接字做成资源池，用于 HTTP 客户端请求。在 HTTP 请求时，如果需要自定义一些代理参数（如：主机的套接字并发数、套接字发送 TCP KeepAlive 包的频率等），可以设置此对象。该对象由构造函数 `new Agent([options])` 创建并返回。

```javascript
const http = require('http')
let keepAliveAgent = new http.Agent({ keepAlive: true })
let options = { agent: keepAliveAgent }
http.request(options, onResponseCB)
```

#### Class: http.Server - 服务器类

`http.Server` 表示一个 HTTP 服务器，该类实例由 `http.createServer()` 创建并返回。该类是 `net.Server` 的子类，除父类中的方法、事件外，`http.Server` 还将添加一些自己的方法、事件等。

创建一个 HTTP 服务器非常简单：

```javascript
let server = http.createServer()
server.on('request', (req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/html' })
  res.end('OK!')
})
server.listen(3000)
```

每次有新连接进入时，服务器对象的 `request` 事件监听器就会被调用。其中，`req` 表示用户请求对象，它是一个 `http.IncomingMessage` 实例；`res` 表示服务器响应对象，它是一个 `http.ServerResponse` 实例。

#### Class: http.IncomingMessage

`IncomingMessage` 对象（实例）可以被 `http.Server` 或 `http.ClientRequest` 创建。

- 在 `http.Server` 中会被传入 `request` 事件的第一个参数中，表示服务器接收到的用户请求对象，可以通过该对象接收用户请求中的数据提交、文件上传等。
- 在 `http.ClientRequest` 中会被传入 `response` 事件的第一个参数中，表示服务器响应对象，可以通过该对象来访问服务器的响应状态、响应头及响应数据等。

`IncomingMessage` 对象是一个可读流，需要基于流接收来自用户请求或服务器响应的数据。

#### Class: http.ServerResponse

`ServerResponse` 对象（实例）会在 HTTP 服务器中创建，并被传递至 `request` 事件监听函数的第二个参数中，服务器通过该对象向用户返回响应数据。

`ServerResponse` 对象是一个可写流，可以将可读流 `.pipe()` 转接给该对象，作为用户响应。

### `https`

HTTPS 是在 TLS/SSL 之上构建的 HTTP 协议，Node.js 中作为一个单独的模块实现。

#### Class: https.Server

`https.Server` 是 `tls.Server` 的子类，与 `http.Server` 具有相同的事件。`https.Server` 对象是一个 HTTPS 服务器，可以通过 `https.createServer()` 方法创建服务器：

```javascript
const https = require('https')
const fs = require('fs')

const options = {
  key: fs.readFileSync('test/fixtures/keys/agent2-key.pem'),
  cert: fs.readFileSync('test/fixtures/keys/agent2-cert.pem')
}

https.createServer(options, (req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/html'})
  res.end('Hello, HTTPS!')
}).listen(8080)
```

#### https.request()

HTTPS 模块还提供了用于请求 HTTPS 服务器的方法 `https.request()` 和 `https.get()`：

```javascript
https.request(options, callback)
```

与 `https.request()` 和 `https.get()` 不同，这两个方法在进行 HTTP 请求时可以添加 TLS/SSL 密钥/证书等：

## 缓存、流与文件

### `buffer` - 缓存

在 ES6 推出 `TypedArray` 之前，JavaScript 并没有读取或操作二进制数据的能力。`Buffer` 为 Node.js 带来了如 TCP 流操作和文件系统流操作的能力。

现在 `TypedArray` 被添加到了 ES6 中，而 `Buffer` 类实现了 `Unit8Array` 相关 API，这样会更加优化和适合在 Node.js 中使用。`Buffer` 实例与整型数非常类似，但其长度是固定的，创建后不能修改。

`Buffer` 在 Node.js 中是一个全局对象，可以像下面这样使用这个类：

```javascript
// 创建一个长度为 10 的缓存
const buf1 = new Buffer(10)
// 创建一个包含 [01, 02, 03] 的缓存
const buf2 = new Buffer([1, 2, 3])
// 创建一个包含 ASCII 编码的包含 [74, 65, 73, 74] 的缓存
const buf3 = new Buffer('test')
// 创建一个 UTF8 编码的包含 [74, c3, a9, 73, 74] 的缓存
const buf4 = new Buffer('tést', 'utf8')
```

#### 缓存区编码

Buffers 通常用来表示经过编码（如：UTF8、UCS2、Base64、Hex 等）的字符序列。可以通过制定编码方式，在 Buffers 和日常使用字符串之间进行转换：

```javascript
const buf = new Buffer('hello world', 'ascii')
```

Node.js 支持的编码方式有：

- 'ascii' - 7 位的 ASCII 数据，这种编码方式非常快，而且会剥离设置过高的位
- 'utf8' - 多字节编码 Unicode 字符，大部分网页和文档使用这类编码方式
- 'utf16le' - 2 个或 4 个字节，Little Endian（LE）编码 Unicode 字符，编码范围（U+100000 到 U+10FFFF）
- 'ucs2' - 'utf16le' 的别名
- 'base64' - Base64 字符串编码
- 'binary' - 将原始二进制数据编码为字符串
- 'hex' - 每个 byte 编码成 2 个十六进制字符

#### Buffers 与 TypedArray

Buffer 同样是一个 Uint8Array 类型数组实例。但是，它与 ECMAScript 2015 的 TypedArray（类型数组）规范并不完全兼容。如：`ArrayBuffer#slice()` 会创建一个分隔部分数据的拷贝，而 `Buffer#slice()` 会创建一个 Buffer 中拷贝数据的视图，相对来说 `Buffer#slice()` 更高效。

Buffer 与类型数组可以使用相同的内存区，使用类型数组对象的 `.buffer` 属性创建缓存即可：

```javascript
const arr = new Uint16Array(2)
arr[0] = 5000
arr[1] = 4000

const buf1 = new Buffer(arr) // <Buffer 88 a0>，buf1.length 为 2
const buf2 = new Buffer(arr.buffer) // <Buffer 88 13 a0 0f>，buf2.length 为 4

console.log(buf1, buf2)
```

#### Class: Buffer

`Buffer` 是一个用于处理二进制数据的全局类，可以通过构造函数创建该类的实例。它有多种构建方式：

```javascript
// 通过一个数组创建
new Buffer(array)
// 通过另一个缓存实例创建
new Buffer(buffer)
// 通过一个类型数组创建
new Buffer(arrayBuffer)
// 创建一个指定长度的缓存对象
new Buffer(size)
// 通过指定字符串（及编码）创建缓存对象
new Buffer(str[, encoding])
```

`Buffer` 类提供了一些类方法，这些方法可以用于比较两个缓存是否相同、连接两个缓存对象等：

- `Buffer.byteLength(string[, encoding])` - 检测字符串编码后的位长度
- `Buffer.compare(buf1, buf2)` - 比较两个缓存对象是否相同
- `Buffer.concat(list[, totalLength])` - 连接多个缓存对象
- `Buffer.isBuffer(obj)` - 检查是否是一个缓存对象
- `Buffer.isEncoding(encoding)` - 检查指定的编码是否可用

### `stream` - 流处理

流（Stream）是一个抽象的接口。Node.js 中流有 4 种形式 Readable（可读流）、Writable（可写流）、Duplex（双向流）和 Transform（转换流）。可以通过 `require('stream')` 来加载流模块，这四种流在该模块中被分别实现为一个单独的类。

`stream` 模块中，刘相关的 API 被实现为两类：面向消费者的 API 和面向实现者的 API。我们更多时候是作为一个流的消费者（使用者），如：从一个可读流中读取数据、向一个可写流中写入数据或将一个可读流的数据转接到可写流中。

Node.js 中有很多对象都是基于流实现，如：HTTP 中用户请求对象 `http.IncomingMessage` 就是一个可读流、服务器响应对象 `http.ServerResponse` 是一个可写流、TCP 和 UDP 套接字 Socket 都被实现为一个双向流。

所有类型的流都实现了 `EventEmitter`，可以在特定的时刻发送一些事件。

#### Class: stream.Readable

`Readable`（可读）流是对正在读取数据的抽象，它表示一个数据的来源，可读流在接收数据前不会发生数据。可读流有**流动**和**暂停**两种模式。我们可以使用 `readable.pause()` 方法暂停一个流动流，或者使用 `readable.resume()` 方法恢复一个暂停流。

和 Linux 等操作系统中的流一样，我们可以使用 `readable.pipe()` 方法将一个可读流，也可以使用 `unpipe()` 方法移除流导向。可读流是一个事件发射器，如：收到数据时会触发 `data` 事件、数据接收完成会收到 `end` 事件、发生错误会触发 `error` 事件。

读取可读流中的数据时可以使用 `readable.read()` 方法从缓冲区中读取数据。但更推荐基于事件进行数据处理：

```javascript
let readable = getReadableStreamSomehow()
readable.on('data', chunk => {
  console.log('Got %d bytes of data.', chunk.length)
})
readable.on('end', () => {
  console.log('There will be no more data.')
})
```

#### Class: stream.Writable

`Writable`（可写）流表示数据写入目标的一个抽象。可写流中也存在一些时间，如：表示可写流已排空的 `drain` 事件、表示操作完成的 `finish` 事件、表示发生错误的 `error` 事件等。

可以使用 `writable.write()` 或 `writable.end()` 方法向可写流中写入数据。Node.js 中所有向可写流中写入数据的方法都是来自于 `Writable`：

```javascript
http.createServer((req, res) => {
  res.write('hello, ')
  res.end('world!')
})
```

#### Class: stream.Duplex

`Duplex` 表示双向流，它实现了 `Readable` 和 `Writable` 两者的接口。

Node.js 中的双向流有：TCP Socket、Zlib 流、crypto 流等。

#### Class: stream.Transform

`Transform`（转换）流是一种输出由输入计算所得的双工流。它们同时实现了 `Readable` 和 `Writable` 接口。Node.js 中的转换流有：Zlib 流、crypto 流。

### `fs` - 文件处理

`fs` 文件系统模块是对标准 POSIX 文件 I/O 操作方法的一个简单包装。可以通过 `require('fs')` 来获取该模块。这个模块中的所有方法均有异步和同步版本。

可以像下面这样，使用异步方法删除一个文件：

```javascript
const fs = reuqire('fs')
fs.unlink('/tmp/hello', err => {
  if (err) throw err
  console.log('Successfully deleted /tmp/hello')
})
```

也可以使用同步方法：

```javascript
const fs = require('fs')
fs.unlinkSync('/tmp/hello')
console.log('Successfully deleted /tmp/hello')
```

对于异步方法来说，需要一个回调函数在操作完成时进行调用。通常情况下回调函数的第一个参数为返回的错误信息，如果异步操作执行正确，则该错误参数为 `null` 或 `undefined`。

如果使用同步版本的方法，一旦出现错误，会以通常的抛出错误的形式返回错误。你可以用 `try_catch` 语句来捕获错误以保证程序的正常运行。

`fs` 模块主要用于文件、目录的操作，以下是一些常用方法。这些方法为异步方法，它们都有对应的同步方法：

- `fs.access(path[, mode], callback)` - 判断用户是否有对指定路径/模式 `path.mode` 的访问权限
- `fs.appendFile(file, data[, options], callback)` - 向指定文件中追加数据
- `fs.chmod(path, mode, callback)` - 修改指定文件/目录的可访问模式
- `fs.chown(path, uid, gid, callback)` - 修改指定文件/目录的所有者
- `fs.link(srcpath, dstpath, callback)` - 建立文件连接
- `fs.mkdir(path[, mode], callback)` - 创建目录
- `fs.open(path, flags[, mode], callback)` - 打开以指定的模式打开文件
- `fs.read(fd, buffer, offset, length, position, callback)` - 读取文件
- `fs.readdir(path, callback)` - 读取目录
- `fs.readFile(file[, options], callback)` - 读取文件全部内容
- `fs.rename(oldPath, newPath, callback)` - 重命名文件/目录
- `fs.rmdir(path, callback)` - 移除目录
- `fs.unlink(path, callback)` - 移除链接
- `fs.write(fd, buffer, offset, length[, position], callback)` - 向文件中写入缓存数据
- `fs.writeFile(file, data[, options], callback)` - 向文件中写入数据（Buffer 或字符串）

#### 文件操作与流

对较大的文件，需要向文件中写入较多内容时，可以基于流进行处理。`fs` 模块中存在以下两种流：`fs.ReadStream` 和 `fs.WriteStream`。

#### fs.ReadStream

可读流，该类对象由 `fs.createReadStream()` 方法创建：

```javascript
const fs = require('fs')

const readStream = fs.createReadStream('/etc/passwd')

readStream.on('open', fd => {
  console.log('文件已打开。')
})

readStream.on('data', data => {
  console.log('收到数据：' + data.toString())
})
```

#### fs.createWriteStream

可写流，该类对象由 `fs.createWriteStream()` 方法创建：

```javascript
const fs = require('fs')

const readStream = fs.createReadStream('/etc/passwd')
const writeStream = fs.createWriteStream('./my.txt')

readStream.on('data', data => {
  writeStream.write(data)
})
```

#### `path` - 路径处理

操作文件时一般都需要处理文件路径，`path` 模块是 Node.js 中用于处理文件路径的模块。它可以帮你规范化连接和解析路径，还可以用于绝对路径到相对路径的转换、提取路径的组成成分及判断路径是否存在。

`path` 模块是一个用于处理和转换文件路径的工具集。几乎所有的方法只做字符串转换，不会调用文件系统检查路径是否有效。

以下是这个模块的一些用法：

使用 `path.basename()` 返回路径的最后一部分：

```javascript
path.basename('/foo/bar/baz/asdf/quux.html') // 'quux.html'
path.basename('/foo/bar/baz/asdf/quux.html', '.html')
```

使用 `path.dirname()` 返回目录名：

```javascript
path.dirname('/foo/bar/baz/asdf/quux.html') // '/foo/bar/baz/asdf'
```

使用 `path.extname()` 返回文件扩展名：

```javascript
path.extname('index.html') // returns '.html'
path.extname('index.coffee.md') // returns '.md'
path.extname('index.') // returns '.'
path.extname('index') // returns ''
path.extname('.index') // returns ''
```

使用 `path.join()` 方法连接两个路径：

```javascript
path.join('/foo', 'bar', 'baz/asdf', 'quux', '..') // returns '/foo/bar/baz/asdf'
path.join('foo', {}, 'bar') // throws exception TypeError: Arguments to path.join must be strings
```

使用 `path.relative()` 方法返回两个路径的相对关系：

```javascript
path.relative('C:\\orandea\\test\\aaa', 'C:\\orandea\\impl\\bbb') // returns '..\\..\\impl\\bbb'
path.relative('/data/orandea/test/aaa', '/data/orandea/impl/bbb') // returns '../../impl/bbb'
```

使用 `path.resolve()` 方法返回绝对路径：

```javascript
path.resolve('/foo/bar', './baz') // returns '/foo/bar/baz'
path.resolve('/foo/bar', '/tmp/file/') // returns '/tmp/file'
```

## 系统与进程、集群

### `os` - 系统信息查看

`os` 模块是一个提供了操作系统查询相关 API 的工具模块，可以通过 `require('os')` 获取模块的引用。

`os` 模块中具有以下操作接口：

- `os.EOL` - 系统行结束符常量，如：`\n`
- `os.arch()` - CPU 架构信息，如：`x64`、`arm`、`ia32`
- `os.cpus()` - 返回 CPU 信息
- `os.endianness()` - 返回 CPU 的字节序，可能 `BE` 或 `LE`
- `os.freemem()` - 返回操作系统空闲内存量，单位字节
- `os.homedir()` - 返回当前用户的主目录
- `os.hostname()` - 返回操作系统主机名
- `os.loadavg()` - 返回一个包含 1、5、15 分钟平均负载的数组
- `os.networkInterfaces()` - 获取一个网络接口的列表信息
- `os.platform()` - 返回操作系统平台，如：`darwin`、`freebsd`、`linux`、`sunos` 或 `win32`
- `os.release()` - 返回操作系统的发行版本
- `os.tmpdir()` - 返回操作系统默认的临时文件目录
- `os.totalmem()` - 返回操作系统内存总量
- `os.type()` - 返回操作系统类型，如：`linux`、`Darwin`、`Windows_NT`
- `os.uptime()` - 返回操作系统运行的事件，单位为秒

### `process` - 进程对象

`process` 表示进程对象，它是一个全局对象，不需要引用即可使用。它还是一个 EventEmitter 实例，我们可以通过它的一些事件监听进程的退出、异常等。

#### 进程中的常用的事件

- Event: 'beforeExit'

这一事件会在 Node.js 清空事件循环，且没有其他调度安排时触发。通常，Node.js 退出时没有工作安排，但 `beforeExit` 事件监听器可以被异步调用，并导致 Node.js 继续运行。

`beforeExit` 并不是在进程明确退出时调用的事件，如：调用 `process.exit()` 方法或发生异常时，其不能作为 `exit` 的替代事件，除非有其他的操作安排。

- Event: 'exit'

会在进程退出时触发的事件。注意：该事件触发后并不能阻止事件循环的退出，一旦 'exit` 事件监听器调用完成后，进程就会退出。

该事件只会在 `process.exit()` 调用后或隐式地结束时间循环后触发。

```javascript
process.on('exit', code => {
  setTimeout(() => {
    console.log('This will not run')
  }, 0)
  console.log('About to exit with code: ', code)
})
```

- Event: 'uncaughtException'

`uncaughtException` 会异常冒泡回归到事件循环中就会触发。默认情况下，发生异常时 Node.js 会向 `stderr` 中打印堆栈信息并结束进程；监听了该事件后，会改变默认处理行为。

注意：`uncaughtException` 是一个非常粗略的异常处理，仅作为异常捕获的最后手段。视图从异常中恢复应用，可能会导致额外的不可预见和不可预知的问题。

#### 进程中的常用属性

`process` 中有很多实用的属性，如：可以使用 `process.env` 查看系统中的环境变量，使用 `process.execArgv 查看当前脚本的执行参数，而 `process.platform` 与 `os.platform()` 一样会返回运行平台信息。

#### 进程中的常用方法

- `process.exit([code])` 该方法会退出当前进程，调用后会触发 `exit` 事件。
- `process.nextTick(callback)` 会在事件循环的下一次循环中调用 `callback` 回调函数。它不是 `setTimeout(fn, 0)` 函数的一个简单别名，因为它的效率更高，能够在任何 I/O 事件之前调用我们的回调函数。但是这个函数在层次上超过某个限制的时候，也会出现问题，可以通过 `process.maxTickDepth` 属性查看执行限制。

#### 基于流的标准输入/输出/错误

操作系统中有 `stdin`（标准输入）、`stdout`（标准输出）、`stderr`（标准错误）三种流，Node.js 这三种流的操作 API 被实现到了 `process` 模块中。

`process.stderr` 输出到 `stderr` 的可写流，`process.stdout` 输出到 `stdout` 的可写流，`process.stderr` 和 `process.stdout` 不同于其他 Node.js 中的可写流，它们通常是阻塞的。`process.stdin` 是一个指定 `stdin` 的可读流。

### `child_process` - 子进程

Node.js 通过 `child_process` 模块提供了类似 `popen(3)` 的三向数据流处理（`stdin`/`stdout`/`stderr`）的功能。它能够以完全非阻塞的方式与子进程的 `stdin`、`stdout`、`stderr` 以流式传递数据。

`child_process` 模块非常有用，我们可以使用它来执行外部命令、实现进程间的通讯等。

如，可以使用 `child_process.spawn()` 来生成一个外部进程，并在其他 Shell 执行命令：

```javascript
const spawn = require('child_process').spawn
const ls = spawn('ls', ['-lh', __dirname])

ls.stdout.on('data', data => {
  console.log(`stdout: ${data}`)
})

ls.on('close', code => {
  console.log(`child process exited with code ${code}`)
})
```

`child_process.spawn()` 还有一个同步版本的方法 `child_process.spawnSync()`。`child_process` 中还有一些其他的常用方法：

- `child_process.exec()`：生成一个 Shell 并在其中执行命令，其回调函数形式为 `function(error, stdout, stderr)`
- `child_process.execFile()`：类似 `child_process.exec()`，但会执行一个文件中的命令
- `child_process.fork()`：生成一个 Node.js 子进程

#### Class: ChildProcess

`ChildProcess` 是 `child_process` 中唯一的类，它是一个 EventEmitter。子进程中有三个关联的流：`child.stdin`、`child.stdout` 和 `child.stderr`。它们可以共享父进程的 `stdio` 流，也可以作为独立的被导流的流对象。

`ChildProcess` 不能直接创建实例，需要通过 `spawn()` 或 `fork()` 来实例化。

可以通过 `child.send()` 实现进程间的通讯：

```javascript
const child = require('child_process').fork('child.js');

// 打开服务器并发送处理句柄
const server = require('net').createServer();
server.on('connection', (socket) => {
  socket.end('handled by parent');
});
server.listen(1337, () => {
  child.send('server', server);
});
```

### `cluster` - 集群

Node.js 是线程运行的语言，其每个实例都运行于单个线程中。这并不能发挥多核服务器的处理能力，这时我们可以借助 `cluster` 模块来启动一个进程集群来处理负载。通过 `cluster` 模块可以很简单地创建子进程，并共享服务器的端口：

```javascript
const cluster = require('cluster')
const http = require('http')
const numCPUs = require('os').cpus().length

if (cluster.isMaster) {
  // 根据 CPU 核数，启动指定数量的进程
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork()
  }
  cluster.on('exit', (worker, code ,signal) => {
    console.log(`Worker ${worker.process.pid} died.`)
  })
} else {
  http.createServer((req, res) => {
    res.writeHead(200)
    res.end('Hello, world!\n')
  }).listen(8080)
}
```

#### 工作原理

在多进程中，有“主进程”和“工作进程”两种角色。工作进程通过 `child_process.fork()` 方法派生，主从进程间可以通过 IPC（进程间通讯）实现进程间的通讯并互相传递服务器句柄。

集群模块支持两种传入连接的分配方式：

- 第一种（除 Windows 外所有平台的缺省方式）为循环方式：主进程监听一个端口，接受新连接以轮询的方式分配给工作进程，并且其内建了一些处理机制来避免单个工作进程的超载。
- 第二种方式是：主进程建立监听套接字，并将它发送给感兴趣的工作进程，由工作进程直接接受传入连接。

`server.listen()` 方法会将大部分工作交给主进程，在这种情况下，一个普通进程和在集群中工作的进程会有以下三种情况：

1. `server.listen({fd: 7})` 由于消息被传递到主进程，父进程中的文件描述符 `7` 会被监听，并且句柄会被传递给工作进程，而不是监听工作进程中文件描述符 `7` 所引用的东西。
2. `server.listen(handle)` 明确一个监听句柄会使得工作进程使用所提供的句柄，而不是与主进程通讯。
3. `server.listen(0)` 通常会让服务器监听一个随机端口。而在集群中，各工作进程每次 `listen(0)` 都会得到同样的“随机”端口。即，使用第一次随机生成的端口。

注意：工作进程间并没有状态的共享，这样你在设计程序时（如：会话、登录等），不能依赖内存对象。这时你可能需要借助 Redis 等外部存储，持久化存储内存中的会话信息。

#### Class: Worker

`Worker` 表示由 `cluster.fork()` 派生的工作进程对象，该对象包含了工作进程的所有公开信息和方法。可通过在主进程中通过 `cluster.workers` 或工作进程中的 `cluster.worker` 获取类实例。它是一个 EventEmitter，如：会在断开连接时收到 `disconnect` 事件、会在工作进程退出时收到 `exit` 事件、而在收到主进程消息时会收到 `message` 事件。

```javascript
const cluster = require('cluster')
const http = require('http')

if (cluster.isMaster) {
  // Keep track of http requests
  var numReqs = 0
  setInterval(() => {
    console.log('numReqs =', numReqs)
  }, 1000)

  // 统计请求数
  function messageHandler (msg) {
    if (msg.cmd && msg.cmd === 'notifyRequest') {
      numReqs += 1
    }
  }

  // 启动工作进程并启动请求监听
  const numCPUs = require('os').cpus().length
  for (var i = 0; i < numCPUs; i++) {
    cluster.fork()
  }

  Object.keys(cluster.workers).forEach((id) => {
    cluster.workers[id].on('message', messageHandler)
  })
} else {
  // 工作进程是一个 HTTP服务器
  http.Server((req, res) => {
    res.writeHead(200)
    res.end('hello world\n')

    // 通过主进程收到请求
    process.send({ cmd: 'notifyRequest' })
  }).listen(8000)
}
```

`Worker` 对象中还有一些方法，使用较多的是 `worker.send(message[, sendHandle][, callback])`，该方法用于向主进程发送消息。

## 加密、编/解码、压缩

### `crypto` - OpenSSL 加密功能

`crypto` 模块提供了加密相关的功能，它封装了 OpenSSL 中的 hash、HMAC、cipher、decipher、sign 和 verify 相关功能。`https` 和 `tls` 中安全凭证相关方法就由此模块提供。

#### Class: Certificate - `SPKAC` 数据处理

SPKAC 是证书签名请求机制，最初由网景公司制定，现已制定为 HTML5 的 `keygen` 元素。`crypto` 模块提供了用于 SPKAC 数据的 `Certificate` 类，其最常使用的地方是用于 HTML5 `<keygen>` 元素的输出。Node.js 内部通过 OpenSSL SPKAC 实现。

可以通过 `new` 关键字，或直接调用 `crypto.Certificate()` 创建该类的实例：

```javascript
const crypto = require('crypto')
const cert1 = new crypto.Certificate()
const cert2 = crypto.Certificate()
```

`certificate.exportChallenge(spkac)` 用于导出口令，`spkac` 的数据结构为一个公钥和一个口令。可以通过 `certificate.exportChallenge(spkac)` 方法导出口令部分；`certificate.exportPublicKey(spkac)` 方法导出公钥部分，其参数可以是一个 Buffer 或一个字符串：

```
const cert = require('crypto').Certificate()
const spkac = getSpkacSomehow()
const challenge = cert.exportChallenge(spkac)
const publicKey = cert.exportPublicKey(spkac)
```

可以通过 `certificate.verifySpkac(spkac)` 方法验证 `spkac` 数据是否有效。

#### Class: Cipher - 数据编码

`Cipher` 对象用于编码数据，它有以下两种用法：

- 作为一个可读或可写流，在写入端写入未加密数据而在可读端生成加密数据
- 使用 `cipher.update()` 和 `cipher.final()` 生成加密数据

`Cipher` 可以使用 `crypto.createCipher()` 和 `crypto.createCipheriv()` 两种方式实例化，同样支持使用或不使用 `new` 关键字。

作为流使用 `Cipher`：

```javascript
const crypto = require('crypto')
const cipher = crypto.createCipher('aes192', '123456')

let encrypted = ''

cipher.on('readable', () => {
  let data = cipher.read()
  if (data) {
    encrypted += data.toString('hex')
  }
})

cipher.on('end', () => {
  console.log(encrypted)
})

cipher.write('some raw text data')
cipher.end()
```

使用 `Cipher` 及流转接：

```javascript
const crypto = require('crypto')
const fs = require('fs')
const cipher = crypto.createCipher('aes192', '123456')

const input = fs.createReadStream('test.js')
const output = fs.createWriteStream('test.enc')

input.pipe(cipher).pipe(output)
```

使用 `cipher.update()` 和 `cipher.final()` 方法：

```javascript
const crypto = require('crypto')
const cipher = crypto.createCipher('aes192', '123456')

let encrypted = cipher.update('some clear text data', 'utf8', 'hex')
encrypted += cipher.final('hex')
console.log(encrypted)
```

#### Class: Decipher - 数据解码

`Decipher` 实例用于数据解码，它有以下两种使用方式：

- 作为一个可读或可写流，在写入端写入加密数据而在可读端生成未加密数据
- 使用 `decipher.update()` 和 `decipher.final()` 生成未加密数据

其使用方法与 `Cipher` 类似。

#### Class: DiffieHellman - Diffie-Hellman 交换密钥

`DiffieHellman` 是一个创建 Diffie-Hellman 交换密钥的工具类。创建一个 `DiffieHellman` 实例，可以使用 `crypto.createDiffieHellman()` 方法。

#### Class: ECDH

`ECDH` 用于生成椭圆曲线 Diffie-Hellman（ECDH）交换密钥。可以使用 `crypto.createECDH()` 创建该类的实例。

#### Class: Hash - 哈希摘要

`Hash` 对象是一个用于计算数据哈希摘要的工具集，它有以下两种使用方式：

- 作为一个可读或可写流，在写入端写入数据而在可读端生成摘要数据
- 使用 `hash.update()` 和 `hash.digest()` 生成摘要数据

可以使用 `crypto.createHash()` 创建该类的实例。

#### Class: Hmac

`Hmac` 用于创建 HMAC 摘要，它有两种使用方式：

- 作为一个可读或可写流，在写入端写入数据而在可读端生成HMAC摘要数据
- 使用 `hmac.update()` 和 `hmac.digest()` 生成HMAC摘要数据

#### Class: Sign - 生成数字签名

`Sign` 用于生成数字签名，它有以下两种使用方式：

- 作为一个可写流，在写入端写入要签名的数据并且 `sign.sign()` 生成签名并返回
- 使用 `sign.update()` 和 `sign.sign()` 生成签名

#### Class: Verify - 验证签名

`Verify` 用于签名验证，其有以下两种使用方式：

- 作为一个可写流，写入的数据是用来验证所提供的签名
- 使用 `verify.update()` 和 `verify.verify()` 验证签名

### `string_decoder` - Buffer 解码

`string_decoder` 模块用于将 Buffer 解码为字符串，它是 `buffer.toString()` 的一个便捷接口，提供对 `utf8` 编码的支持。

#### Class: StringDecoder

可以通过构造函数创建 `StringDecoder` 类的实例，实例化时可以同时指定编码方式，默认编码为 `uft8`。

```javascript
const StringDecoder = require('string_decoder').StringDecoder
const decoder = new StringDecoder('utf8')

const cent = new Buffer([0xC2, 0xA2])
console.log(decoder.write(cent))
```

### `zlib` - 压缩/解压缩

`zlib` 用于流压缩，它提供了对 Gzip/Gunzip、Deflate/Inflate 和 DeflateRaw/InflateRaw 类的绑定，这些类具体有相同的选项，其本身也是一个流。压缩/解压缩时可以通过 `.pipe()` 转接实现，可以将一个 `fs.createReadStream()` 转接到 zlib 流再转接到一个 `fs.createWriteStream()`：

```javascript
const gzip = require('zlib').createGzip()
const fs = require('fs')

const inp = fs.createReadStream('test.js')
const out = fs.createWriteStream('test.gz')

inp.pipe(gzip).pipe(out)
```

压缩/解压缩数据还可以通过方法 `zlib.deflate()` 和 `zlib.unzip()` 便捷地来完成。

在 HTTP 客户端或服务器中，可以通过 `accept-encoding` 请求头或 `content-encoding` 响应头判断数据编码，再根据编码类型进行数据逇解码。

## 辅助模块 URL 处理、工具类等

### `url` - URL 解析

`url` 是 Node.js 提供的用于 URL 解析的工具模块，可以将 URL 中的各部分解析为一个对象，或将指定的字符串参数转化为一个 URL。

该模块包含以下三个方法：

- `url.parse(urlStr[, parseQueryString][, slashesDenoteHost])`：将 URL 字符串解析为对象
- `url.format(urlObj)`：将对象格式化为 URL 字符串
- `url.resolve(from, to)`：URL 路径处理

### `util` - 工具类

`util` 模块中有一些常用的工具函数，其设计目的是满足 Node 内部 API 的使用。但其内部的一些方法，在我们日常编程中也很有用。

`util.format(format[, ...])`：格式化字符串。根据第一个参数格式化字符串，类似于 `printf` 格式化输出。其第一个参数中可能包含零个或多个占位符，支持的占位符有：

- `%s` - 字符串
- `%d` - 数字（整型和浮点型）
- `%j` - JSON，如果这个参数包含循环对象的引用，将会被替换为字符串 '[Circular]'
- `%%` - 单独的（'%'）符号，不会消耗一个参数

`util.inherits(contructor, superConstructor)`：实现继承。通过构造函数，继承对象上的方法。会从父类的构造函数 `superConstructor` 创建一个新对象 `constructor`。

`util.inspect(object[, options])`：序列化对象。将对象以字符串的形式显示，可以通过 `showHidden` 参数设置是否显示隐藏属性，通过 `depth` 设置显示深度。

## 模块、全局对象

### `global` - 全局对象

全局对象不需要 `require` 引用即可在所有模块中使用。在 Node.js 中，通过 `var` 定义的变量只属于那个模块，要添加全局变量，需要将其添加到 `global` 命名空间中。

Node.js 中的全局对象有：

- Class: Buffer，缓存类，用于处理二进制数据
- __dirname，脚本目录，当前执行脚本所在目录的目录名
- __filename，脚本路径，当前执行脚本的完整路径
- timers，定时器，Node.js 中的定时器函数来自于 JavaScript，分别是：`clearImmediate()`、`clearInterval()`、`clearTimeout()`、`setImmediate()`、`setInterval()` 和 `setTimeout()` 等。
- console，控制台
- epxorts，对 `module.exports` 的一个引用，用于模块导出
- require，引用模块，引用后可访问 `exports` 导出的内容
