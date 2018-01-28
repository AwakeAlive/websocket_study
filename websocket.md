## websocket

### websocket定义：
浏览器发起一个`websocket`请求，服务器接收`websocket`请求后，
它会在服务器和浏览器之间建立一个`web`端的`socket`连接。
`socket`连接允许浏览器和服务器相互发送消息。

浏览器和服务器两者都不断开的话，`socket`会一直都在。
`websocket`本质上是TCP连接。

简单示例：
```html
<h1>Echo test</h1>
<input type="text" id="sendTxt" name="" value="">
<button id="sendBth">send</button>
<div id="receiveView"></div>
```

```js
let websocket = new WebSocket("ws://echo.websocket.org/") // 建立websocket连接

websocket.onopen = function() { // open
  console.log('websocket connected');
  document.getElementById('receiveView').innerHTML = 'connected';
}

websocket.onclose = function() {
  console.log('websocket closed'); // close
  document.getElementById('receiveView').innerHTML = 'closed';
}

websocket.onmessage = function(e) {
  console.log('websocket message: ', e.data) // 接收消息
  document.getElementById('receiveView').innerHTML = e.data
}

document.getElementById('sendBth').onclick = function() {
  var sendTxt = document.getElementById('sendTxt').value;
  websocket.send(sendTxt) // 发送消息
}
```
运行Chrome环境，图片示例
![websocket](/websocket1.png)
![websocket](/websocket2.png)
![websocket](/websocket3.png)

### 搭建自己websocket
github: nodejs-websocket
```js
npm install nodejs-websocket
```

新建`wsServer.js`文件

```js
var ws = require("nodejs-websocket")
var PORT = 3000;
// Scream server example: "hi" -> "HI!!!"
var server = ws.createServer(function (conn) {
	console.log("New connection")
	conn.on("text", function (str) {
		console.log("Received "+str)
		conn.sendText(str.toUpperCase()+"!!!")
	})
	conn.on("close", function (code, reason) {
		console.log("Connection closed")
	})
  conn.on("error", function(err) {
    console.log('handle err');
    console.log(err);
  })
}).listen(PORT)

console.log('websocket server is listening at port:' + PORT)
```

`node wsServer.js `建立`websocket`连接

websocket.html需要修改
```js
let websocket = new WebSocket("ws://localhost:3000/")
```
