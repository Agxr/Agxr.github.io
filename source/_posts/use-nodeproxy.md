---
title: use-nodeproxy
date: 2018-05-02 17:58:25
tags: node-proxy
---

### 本文将介绍用nodejs搭建本地http服务器，并且判断访问接口URL时进行转发，完美解决本地开发时候的跨域问题。

- 首先放两篇借用参考文章----\\\\参考文章1：[node.js搭建本地http服务器参考了shawn.xie的](http://www.cnblogs.com/shawn-xie/archive/2013/06/06/3121173.html)-----------------\\\\[参考文章2](http://www.jb51.net/article/91627.htm)

#### 项目开始：
- 第一步：npm初始
>   npm init

- 第二步：安装node-http-proxy模块
>   npm install http-proxy --save-dev

<!-- more -->

- 第三步：项目的结构
![nodeProxy项目结构](./node-proxy-project.png)

- 第四步：proxy.js
```javascript
var http = require('http'),
    url = require('url'),
    path=require('path'),
    fs=require('fs'),
    httpProxy = require('http-proxy'),
    mine=require('./mine').types;

//
// Create a proxy server with latency
//
var proxy = httpProxy.createProxyServer({
    //target: 'http://xptest.tianxialashou.com.cn',   //接口地址
    target: 'https://api.douban.com',   // 豆瓣api测试接口地址
    // 下面的设置用于https
    // ssl: {
    //     key: fs.readFileSync('server_decrypt.key', 'utf8'),
    //     cert: fs.readFileSync('server.crt', 'utf8')
    // },
    // secure: false
});

//
// Listen for the `proxyRes` event on `proxy`.
//
proxy.on('proxyRes', function (proxyRes, req, res) {
  console.log('RAW Response from the target', JSON.stringify(proxyRes.headers, true, 2));
});

//
// Listen for the `open` event on `proxy`.
//
proxy.on('open', function (proxySocket) {
  // listen for messages coming FROM the target here
  proxySocket.on('data', hybiParseAndLogMessage);
});

//
// Listen for the `close` event on `proxy`.
//
proxy.on('close', function (res, socket, head) {
  // view disconnected websocket connections
  console.log('Client disconnected');
});


var PORT = 3000;

//
// Create your server that makes an operation that waits a while
// and then proxies the request
//

var server = http.createServer(function (request, response) {
    var pathname = url.parse(request.url).pathname;
    //var realPath = path.join("main-pages", pathname); // 指定根目录
    var realPath = path.join("./", pathname);
    console.log(pathname);
    console.log(realPath);
    var ext = path.extname(realPath);
    ext = ext ? ext.slice(1) : 'unknown';

    //判断如果是接口访问，则通过proxy转发
    if(pathname.match(/\/v2\//)){
        delete request.headers.host;
        proxy.web(request, response);
        // response.write("alksjdfklll-----------------------------------?????");
        // response.end();
        return;
    }

    fs.exists(realPath, function (exists) {
        if (!exists) {
            response.writeHead(404, {
                'Content-Type': 'text/plain'
            });

            response.write("This request URL " + pathname + " was not found on this server.");
            response.end();
        } else {
            fs.readFile(realPath, "binary", function (err, file) {
                if (err) {
                    response.writeHead(500, {
                        'Content-Type': 'text/plain'
                    });
                    response.end(err);
                } else {
                    var contentType = mine[ext] || "text/plain";
                    response.writeHead(200, {
                        'Content-Type': contentType
                    });
                    response.write(file, "binary");
                    response.end();
                }
            });
        }
    });
});
server.listen(PORT);
console.log("Server runing at port: " + PORT + ".");
```
- 第五步：mine.js
```javascript
/**
 * Created by XXX on 2017/9/4.
 */
exports.types = {
    "css": "text/css",
    "gif": "image/gif",
    "html": "text/html",
    "ico": "image/x-icon",
    "jpeg": "image/jpeg",
    "jpg": "image/jpeg",
    "js": "text/javascript",
    "json": "application/json",
    "pdf": "application/pdf",
    "png": "image/png",
    "svg": "image/svg+xml",
    "swf": "application/x-shockwave-flash",
    "tiff": "image/tiff",
    "txt": "text/plain",
    "wav": "audio/x-wav",
    "wma": "audio/x-ms-wma",
    "wmv": "video/x-ms-wmv",
    "xml": "text/xml",
    "woff": "application/x-woff",
    "woff2": "application/x-woff2",
    "tff": "application/x-font-truetype",
    "otf": "application/x-font-opentype",
    "eot": "application/vnd.ms-fontobject"
};
```

- 第六步：html页面调用ajax
```javascript
  $.ajax({
    type: "GET",
    url: "/v2/movie/subject/1764796",
    success: function(response){
      console.log(response)
    }
  })
```