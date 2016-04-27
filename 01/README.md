## 初识 Express 

### 安装 Express 应用生成器

Express 是一个基于 Node.js 的 Web 开发框架，我们可以使用它快速开发一个 Web 应用。

我们可以使用 Express 的应用生成器快速生成一个 Express 项目。

输入下面的命令来安装 Express 应用生成器：

```
$ npm install express-generator -g
```

-h 参数可以列出所有可用的命令选项：

```
$ express -h

  Usage: express [options] [dir]

  Options:

    -h, --help          output usage information
    -V, --version       output the version number
    -e, --ejs           add ejs engine support (defaults to jade)
        --hbs           add handlebars engine support
    -H, --hogan         add hogan.js engine support
    -c, --css <engine>  add stylesheet <engine> support (less|stylus|compass|sass) (defaults to plain css)
        --git           add .gitignore
    -f, --force         force on non-empty directory
```

### 创建一个简单的 Express 应用

上面提到，我们可以使用 Express 应用生成器来快速生成一个 Express 项目，如下，我们来创建一个名为 `blog` 的项目：

```
$  express -e blog

   create : blog
   create : blog/package.json
   create : blog/app.js
   create : blog/public
   create : blog/routes
   create : blog/routes/index.js
   create : blog/routes/users.js
   create : blog/public/javascripts
   create : blog/views
   create : blog/views/index.ejs
   create : blog/views/error.ejs
   create : blog/public/stylesheets
   create : blog/public/stylesheets/style.css
   create : blog/public/images
   create : blog/bin
   create : blog/bin/www

   install dependencies:
     $ cd blog && npm install

   run the app:
     $ DEBUG=blog:* npm start
```

这样，我们的 Express 项目就生成好了。

这里我们使用的 `-e` 参数，意思是使用 `ejs` 模板引擎。因为 Express 默认的模板引擎是 `jade`。`ejs` 模板就是在 HTML 里面使用 `<%` 和 `%>` 来来包含可执行的 `JavaScript` 代码。

首先 EJS 解释器会将 `ejs` 模板里所有 `<%` 和 `%>` 标签内的 `JavaScript` 解释执行，解释完毕后，`ejs` 里面就不含任何 JavaScript 代码，就只有 HTML 文本，然后再将 HTML 文本输出到客户端，如浏览器。

接下来需要安装 Express 项目所需要的依赖包：

```
$ cd blog &&  npm install
```

安装上所有依赖后，我们就可以启动刚创建的项目了：

```
$ DEBUG=blog:* npm start  
```

接下来，就可以在浏览器中输入 `localhost:3000`，来看看刚创建的项目。


### 工作原理

可以看到，浏览器中会输出 Express 的欢迎信息。

当我们访问 `localhost:3000` 的时候，其实是访问的 `lcoalhsot:3000` 这个 Domain 下的 `/` 路由。

这个路由是定义在 `routes/index.js` 里面的，其中有这样的代码：

```
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});
``` 

它的意思就是，监听访问 '/' 路由的 GET 请求，如果监听到改请求，就执行一个回调函数。

回调函数有三个参数，一个是 `req` ，即 `requrest`，表示请求对象；一个是 `res`，即 `response`，响应对象；还有一个是 `next` ，它是处于请求-响应循环流程中的中间件，当当前中间件没有终结请求-响应循环，必须调用 next() 方法将控制权交给下一个中间件。

在回调函数里面，使用 `res` 对象的 `render()` 方法，来渲染一个前端模板。

`render()` 方法有两个参数，一个是模板名称，这里为 `index`，还一个参数是一个对象，这个对象是要传递给前端模板的数据。


### 模板

上面提到的 `index` 模板即为 `views/index.ejs`，打开会发现里面有这样几行代码：

```
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1><%= title %></h1>
    <p>Welcome to <%= title %></p>
  </body>
</html>
```

首先 `<%` 和 `%>` 标签里面是 JavaScript 变量，我们可以使用 `<%=  变量名 %>` 的方式来输出变量。该段代码输出的就是 `title` 这个变量，这个变量是 `routes/index.js`  里面通过 `res.render()` 方法输出的，且变量的值为 `Express`，所以我们访问 `localhost:3000` 的时候，会在浏览器中看到 `Express` 字样。

ejs 的标签系统非常简单，它只有以下三种标签：

+ `<% code %>`  JavaScript 代码。
+ `<%= code %>` 显示替换过 HTML 特殊字符的内容。
+ ` <%- code %>` 显示原始 HTML 内容。

### Hello， World

既然知道原理了，那么接下来就可以自己修改一下 `title` 变量或 `ejs` 模板文件内容，比如将 `title ` 改为 `Hello, World`。

每次改了后端代码之后记得要重启应用，才能看到效果。

按两次 `ctrl+c` 退出，然后输入 `DEBUG=blog:* npm start` 再次启动应用。

### 项目结构

现在我们再来看看生成的项目目录里面都有什么：

```
bin/                              
node_modules/               
public/                             	
routes/                            
views/                             	
app.js					
package.json			
```

+ **bin/** 可执行文件
+ **node_modules/** 手动安装的 node.js 的依赖包
+ **public/** 存放 css, js, img 等静态文件
+ **routes/** 路由模板文件
+ **views/** 模板文件	 
+ **app.js** 入口文件
+ **package.json** 存储项目信息及依赖模块。当执行 `npm install` 时，npm 会安装 `dependencies` 里面定义的依赖包

### app.js

接下来我们看看 `app.js` 文件里面有什么：

```
var express = require('express');
var path = require('path');
var favicon = require('serve-favicon');
var logger = require('morgan');
var cookieParser = require('cookie-parser');
var bodyParser = require('body-parser');

var routes = require('./routes/index');
var users = require('./routes/users');

var app = express();

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');

// uncomment after placing your favicon in /public
//app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')));
app.use(logger('dev'));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.use('/', routes);
app.use('/users', users);

// catch 404 and forward to error handler
app.use(function(req, res, next) {
  var err = new Error('Not Found');
  err.status = 404;
  next(err);
});

// error handlers

// development error handler
// will print stacktrace
if (app.get('env') === 'development') {
  app.use(function(err, req, res, next) {
    res.status(err.status || 500);
    res.render('error', {
      message: err.message,
      error: err
    });
  });
}

// production error handler
// no stacktraces leaked to user
app.use(function(err, req, res, next) {
  res.status(err.status || 500);
  res.render('error', {
    message: err.message,
    error: {}
  });
});


module.exports = app;

```

首先我们通过  `require()` 引入了一些 Node.js 的核心包和第三方依赖包。

然后通过 `var routes = require('./routes/index');` 和 `var users = require('./routes/users');` 引入了两个路由。

`var app = express()`  生成一个express实例 app。

`app.set('views', path.join(__dirname, 'views’))` 设置 views 文件夹为存放视图文件的目录, 即存放模板文件的地方,__dirname 为全局变量,存储当前正在执行的脚本所在的目录。

`app.set('view engine', 'ejs’)` 设置视图模板引擎为 ejs。

`app.use(favicon(__dirname + '/public/favicon.ico’))` 设置/public/favicon.ico为favicon图标。

`app.use(logger('dev’))` 加载日志中间件。

`app.use(bodyParser.json())` 加载解析json的中间件。

`app.use(bodyParser.urlencoded({ extended: false }))` 加载解析urlencoded请求体的中间件。

`app.use(cookieParser())` 加载解析cookie的中间件。

`app.use(express.static(path.join(__dirname, 'public')))` 设置public文件夹为存放静态文件的目录。

`app.use('/', routes);` 和 `app.use('/users', users)` 是加载路由控制器。

```
// 开发环境下的错误处理器，将错误信息渲染error模版并显示到浏览器中
if (app.get('env') === 'development') {
    app.use(function(err, req, res, next) {
        res.status(err.status || 500);
        res.render('error', {
            message: err.message,
            error: err
        });
    });
}
```

```
// 生产环境下的错误处理器，不会将错误信息泄露给用户
app.use(function(err, req, res, next) {
    res.status(err.status || 500);
    res.render('error', {
        message: err.message,
        error: {}
    });
});
```

`module.exports = app` 导出app实例供其他模块调用。


###  bin/www 

```
#!/usr/bin/env node

var app = require('../app');
```

`#!/usr/bin/env node ` 说明是一个 node 的可执行文件。

`var app = require('../app');` 引入我们刚才导出的 `app.js` 文件。

```
// 这两行代码是设置端口
// 如果环境变量设置了 port，那么就用环境变量的 port；否则使用 3000 端口
var port = normalizePort(process.env.PORT || '3000');
app.set('port', port);
```

比如执行 `PORT=3001 npm start` ，则使用 3001 端口。

```
/**
 * Create HTTP server.
 */

var server = http.createServer(app);

/**
 * Listen on provided port, on all network interfaces.
 */

server.listen(port);
server.on('error', onError);
server.on('listening', onListening);
```

`var server = http.createServer(app);` 启动一个 HTTP 服务，这里使用的 Node.js 自带的 `http` 模块。接下来的几行就是监听端口。

###  路由控制

让我们再回到 `routes/index.js`。我们已经知道 `router.get('/', function(){})` 是监听的 `/` 路由。如果我们在浏览器中输入 `localhost:3000/test`，就会出现 `Not Found` 的提示。这是因为我们没有监听 '/test' 这个路由。

接下来在 `routes/index.js` 添加一段监听 '/test' 路由的代码：

```
router.get('/test', function(req, res, next) {
  res.render('test', { title: 'Hello World' });
});
```

然后在 `views` 目录下新建一个 `test.ejs`：

```
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1><%= title %></h1>
    <p>Welcome to <%= title %></p>
  </body>
</html>
```

再重启应用访问 `localhost:3000/test`，就会发现输出了 `test.ejs` 模板的内容。

express 封装了多种 http 请求方式，我们主要只使用 get 和 post 两种，即 `router.get()` 和 `router.post()`。

第一个参数都为请求的路径，第二个参数为处理请求的回调函数，回调函数有两个参数分别是 req 和 res，代表请求信息和响应信息 。路径请求及对应的获取路径有以下几种形式：

+ **req.query** 处理 get 请求，获取 get 请求参数
+ **req.params** 处理 `/:xxx` 形式的 get 或 post 请求，获取请求参数
+ **req.body** 处理 post 请求，获取 post 请求体
+ **req.param()** 处理 get 和 post 请求，但查找优先级由高到低 `req.params→req.body→req.query`


路由还支持正则表达式，更多路由详情参见 [Express 文档-路由](http://www.expressjs.com.cn/guide/routing.html)














