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


### 路由

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










