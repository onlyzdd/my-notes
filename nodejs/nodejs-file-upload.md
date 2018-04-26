# Nodejs HTTP 服务器实现文件上传

在 HTTP 协议中，`multipart/form-data` 格式用于向服务器发送二进制数据，通过这一内容类型（`Content-Type`）可以实现文件上传。由于这种格式发送的是二进制数据，在服务器端接收和处理数据时会与其他内容类型有所区别。

## HTTP 协议中的文件上传

最早的 HTTP 协议是不支持的文件上传的，在 1995 年制定的 RFC1867 规范中，在 HTTP POST 请求的内容类型 `Content-Type` 中扩展了 `multipart/form-data` 类型，该类型用于向服务器发送二进制数据，以便支持文件的上传。

### POST 上传文件

我们通过 `form` 表单提交文件时，会构造类似下面这样一个表单：

```html
<form enctype="multipart/form-data" action="_URL_" method="POST">
  <input type="file" name="userfile">
  <input type="submit" value="上传文件">
</form>
```

在使用 `form` 提交表单数据时，默认的编码格式为 `application/x-www-form-urlencoded`，上传文件时需要通过 `enctype` 属性将编码方式设置为 `multipart/form-data`。

### HTTP 数据提交与服务器数据解析

HTTP 协议使用 ASCII 传输数据，HTTP 请求中包含三部分：状态行、请求头、请求体。所有 HTTP 请求方法中，都包含状态行、请求行两部分，只有包含数据提交的请求方法（如 `PUT`、`POST`）中才会有请求体部分。

#### 客户端数据发送

在包含请求体的请求中，提交的数据会按指定编码类型进行编码，而客户端会按编码方式设置请求头中的 `Content-Type` 字段。

在一个 `application/x-www-form-urlencoded` 编码的请求中，会设置一个如下的请求头：

```http
Content-Type:applcation/x-www-form-urlencoded
```

而用于文件上传的编码方式 `multipart/form-data`，会设置一个如下的请求：

```http
Content-Type:multipart/form-data, boundary=AaB03x
```

#### 服务器数据接收与解析

对于一个编码方式为 `application/x-www-form-urlencoded` 的请求来说，会对提交内容进行 URL 编码。服务器会接收到类似如下的内容：

```http
POST / HTTP/1.1
Content-Type:application/x-www-form-urlencoded
Accept-Encoding:gzip, deflate
Host:itbilu.com
Content-Length:23
Connection:Keep-Alive
Cache-Control:max-age=0

key1=value1&key2=value2
```

请求头与请求体之间会有一个空行，服务器会对请求体以 queryString 的方式进行解码。

而对于一个 `multipart/form-data` 的文件上传请求来说，收到的内容类似如下：

```http
POST / HTTP/1.1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryYN9YYwO9ESipYBIx
Accept-Encoding: gzip, deflate
Host: itbilu.com
Content-Length: 22646
Connection: Keep-Alive
Cache-Control: max-age=0

------WebKitFormBoundaryoqBx9oYBhx4SF1YQ
Content-Disposition: form-data; name="myName"

itbilu.com
------WebKitFormBoundaryYN9YYwO9ESipYBIx
Content-Disposition: form-data; name="upload"; filename="41GiLecHO3L.jpg"
Content-Type: image/jpeg

����JFIF��C // 文件的二进制数据
……
--------WebKitFormBoundaryYN9YYwO9ESipYBIx--
```

在请求头的 `Content-Type` 字段中，除了编码类型为 `multipart/form-data` 描述外，还有一个 `boundary` 属性，这是客户端随机生成的一个数据边界描述。

如上所示，文件上传时内容是分段传输的，每一 `boundary` 表示一个 `field`（`form` 表单控值）边界。

如上面示例所示，上传文件时除内容描述外还包含一个的 `Content-Type` 文件 `MIME` 的描述，其后是一个空行和文件的二进制数据。所有的表单数据结束后，会有一个 `"--"+boundary+"--"` 结束符。而服务器接收到数据后，同样会根据 `boundary` 来进行数据的接收和解析。

## Nodejs 中处理文件上传

Node.js 中处理文件上传的第三方模块，也可以使用 `formidable` 模块处理文件上传，下面简单介绍使用 `Node.js` 原生环境处理图片上传，上传文件时也可以参考处理。

首先，使用 Node.js 的 HTTP 模块创建一个 HTTP 服务器：

```javascript
var http = require('http')
var fs = require('fs')
var util = require('util')
var querystring = require('querystring')

http.createServer(function (req, res) {
  if (req.url === '/upload' && req.method.toLowerCase() === 'get') {
    res.writeHead(200, { 'Content-Type': 'text/html' })
    res.end(
      '<meta charset="utf-8">' + 
      '<form action="/upload" enctype="multipart/form-data" method="post">'+
        '<input type="file" name="upload" multiple="multiple" />'+
        '<input type="submit" value="Upload" />'+
      '</form>'
    )
  } else if (req.url === '/upload' && req.method.toLowerCase() === 'post') {
    if (req.headers['content-type'].indexOf('multipart/form-data') !== -1) {
      parseFile(req, res)
    }
  } else {
    res.writeHead(200, { 'Content-Type': 'text/html' })
    res.end('Try <a href="/upload">/upload</a>')
  }
}).listen(3000)

/**
 * 解析文件
 * @param {http.IncomingMessage} req 请求
 * @param {http.ServerResponse} res 响应
 */
function parseFile(req, res) {
  var body = ''
  var fileName = ''
  var boundary = req.headers['content-type'].split('; ')[1].replace('boundary=', '')
  req.on('data', function (chunk) {
    body += chunk
  })
  req.on('end', function() {
    var file = querystring.parse(body, '\r\n', ':')
    req.setEncoding('binary')
    if (file['Content-Type'].indexOf('image') !== -1) {
      var fileInfo = file['Content-Disposition'].split('; ')
      for (value in fileInfo) {
        if (fileInfo[value].indexOf('filename=') !== -1) {
          fileName = fileInfo[value].substring(10, fileInfo[value].length - 1)
          if (fileName.indexOf('\\') !== -1) {
            fileName = fileName.substring(fileName.lastIndexOf('\\') + 1).toString('utf-8')
          }
          console.log('文件名：' + fileName)
        }
      }
      var entireData = body.toString()
      var contentTypeRegex = /Content-Type: image\/.*/
      contentType = file['Content-Type'].substring(1)
      var upperBoundary = entireData.indexOf(contentType) + contentType.length
      var shorterData = entireData.substring(upperBoundary)
      var binaryDataAlmost = shorterData.replace(/^\s\s*/, '').replace(/\s\s*$/, '')
      var binaryData = binaryDataAlmost.substring(0, binaryDataAlmost.indexOf('--'+boundary+'--'))
      res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' })
      fs.writeFile(fileName, binaryData, 'binary', function(err) {
        res.end('图片上传完成');
      });
    } else {
      res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' })
      res.end('只能上传图片！')
    }
  })
}
```

在这一步中，我们创建 HTTP 服务器，当 `GET` 请求时，会加载一上用于文件上传的 `form` 表单。上传文件会通过 `POST` 方式提交到服务器，这时服务端会通过 `parseFile` 函数解析并保存文件。

`req` 是一个 `IncomingMessage` 对象，而该对象又实现了 `ReadableStream` ，所以我们可以用流的方式来接收数据。数据接收完成了，按 `RFC1867` 规范进行了数据处理，并通过 `fs` 模块保存了文件。
