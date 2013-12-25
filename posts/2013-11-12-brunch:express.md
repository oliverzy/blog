## Brunch自动重启Express服务器
Brunch - <http://brunch.io>是一个轻量级前端构建工具，它主要用来构建HTML5 Web应用。   
Brunch自带了一个简单服务器，如果你的应用仅有前端的话，那就足够了，但是如果你需要一些后端Restful服务，那么需要写自己的后端了。  

对于快速原型项目，使用一个Java后端就有点太重量级了。这个时候写一个基于NodeJS平台的Express服务器是非常合适的。 nodemon是一个工具能够在代码变化时自动重新启动Express服务器。这篇文章将会指导你如何在brunch中使用nodemon来启动Express服务器。

<!--more-->

假设你的服务器放在server/server.js中，那么你将需要两个命令行分别启动brunch和nodemon，比较麻烦。下面这段简单脚本（nodemon-wrapper.js）能够利用brunch自带的server选项来启动我们自己的服务器。

```
exports.startServer = function(port, path, callback) {
  var child_process = require('child_process');
  var server = child_process.spawn('nodemon', ['server.js'], {cwd: 'server'});
  server.stdout.setEncoding('utf8');
  callback();
  server.stdout.on('data', function(data) {
      process.stdout.write(data);
    });

  server.stderr.on('data', function(data) {
      process.stdout.write(data);
    });

  return server;
};
```

然后在brunch的config.coffee中加入下面一段就行了  

```
server:
    path: 'nodemon-wrapper.js'
```