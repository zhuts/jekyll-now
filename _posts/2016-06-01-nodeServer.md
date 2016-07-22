---
layout: post
title: "Node's HTTP module: Setting up a server with vanilla Node"
categories: JavaScript Node.js back-end
---

If you want a lightweight JavaScript-powered server you can't really do better than vanilla Node.js, which is fully-capable of running a robust, fully-featured server, and implementing it is relatively straight forward.

```javascript
var http = require("http");

var port = 3000;
var ip = "127.0.0.1";

var server = http.createServer(
	function(request, response){
		response.end('You just sent me' + request.body);
	}
)
console.log("On http://" + ip + ":" + port);

server.listen(port, ip);

```
