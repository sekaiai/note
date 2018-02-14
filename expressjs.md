
### cheerio
Fast, flexible & lean implementation of core jQuery designed specifically for the server.
```
# https://github.com/cheeriojs/cheerio#sponsors
const cheerio = require('cheerio')
const $ = cheerio.load('<h2 class="title">Hello world</h2>')

$('h2.title').text('Hello there!')
$('h2').addClass('welcome')

$.html()
//=> <h2 class="title welcome">Hello there!</h2>
```


---
type: nodejs
---
## 项目目录 `process.cwd()`
`process` 项目目录，还有好多搞不懂诶
`process.cwd()` 相当于 `C:\wamp\www\apps\node.js\myapp`


## 文件系统 `const fs = require('fs');`

```
const fs = require('fs');
```

### 写入文件 `fs.writeFile(file, data[, options], callback)`
将字符串写入到文件文件中，是非常简单的操作，使用writeFile即可搞定：
```
fs.writeFile('./test.txt', data, function(err){
      if(err) throw err;
      console.log('写入数据成功...');
});
```

### 读取文件 `fs.readFile(path[, options], callback)`
异步地读取一个文件的全部内容。 例子：
```
fs.readFile('/etc/passwd', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

### 读取文件夹 `fs.readdir(path[, options], callback)`
读取一个目录的内容。 回调有两个参数 `(err, files)`，其中 `files` 是目录中不包括 `'.'` 和 `'..'` 的文件名的数组,`options` 就是单纯的编码格式。
```
fs.readdir('./public/pixiv', (err, files) => {
   if(err) throw err;     
    res.send(files) 
    //["daily-20170417-1.json","daily-20170418-1.json","female-20170424-1.json"]
})
```

### 重命名 `fs.rename(oldPath, newPath, callback)`
在文件系统中，有一个`fs.rename`的方法，顾名思义，对文件（文件夹）进行重命名。
将 `oldPath` 文件（目录）移动至 `newPath` 的路径下，并重新命名；如果 `oldPath` 和 `newPath` 是同一个路径，则直接进行重命名。

### 异步删除 `fs.unlink(path, callback)`
```
const fs = require('fs');

fs.unlink('/tmp/hello', (err) => {
  if (err) throw err;
  console.log('成功删除 /tmp/hello');
});
```


### 判断文件是否存在  `fs.stat` `fs.access`
不推荐使用判断文件是否存在的代码，为啥我也不知道吖。
`fs.stat(path, callback)`
```
fs.stat('./test.js', function(err, stats){
    if( err ){
        console.log( '路径错误' );
        throw err;
    }
    console.log(stats);
    console.log( 'isfile: '+stats.isFile() ); // 是否为文件
    console.log( 'isdir: '+stats.isDirectory() ); // 是否为文件夹
});
```

`fs.access(path[, mode], callback)`
```
fs.access('/etc/passwd', fs.constants.R_OK | fs.constants.W_OK, (err) => {
  console.log(err ? 'no access!' : 'can read/write');
});
```
* `fs.constants.F_OK` - `path` 文件对调用进程可见。 这在确定文件是否存在时很有用，但不涉及 `rwx` 权限。 如果没指定 `mode`，则默认为该值。
* `fs.constants.R_OK` - `path` 文件可被调用进程读取。
* `fs.constants.W_OK` - `path` 文件可被调用进程写入。
* `fs.constants.X_OK` - `path` 文件可被调用进程执行。 对 `Windows` 系统没作用（相当于 `fs.constants.F_OK`）。
* 
### 添加文件夹
```
let $path = './public/pixiv/newfile/'
try {
    fs.accessSync($path, fs.constants.R_OK | fs.constants.W_OK)
} catch (e) {
    fs.mkdirSync($path, '0777')
}
```


## 加密解密 `crypto `
`crypto` 模块提供了加密功能，包含对 OpenSSL 的哈希、HMAC、加密、解密、签名、以及验证功能的一整套封装。

使用 `require('crypto')` 来访问该模块。

### 带 `key` 的加密 `crypto.createHmac(algorithm, key)`

```
const crypto = require('crypto');

const secret = 'abcdefg';
const hash = crypto.createHmac('sha256', secret)
                   .update('I love cupcakes')
                   .digest('hex');
console.log(hash);
```

### 不带 `key` 加密  `crypto.createHash(algorithm)`
支持加密：sha1, sha256, sha512, md5 

```
var crypto = require('crypto');
var name = 'braitsch';

var hash = crypto.createHash('md5').update(name).digest('hex');

console.log(hash)

# 文件加密吧，暂时还没搞懂。
const filename = process.argv[2];
const crypto = require('crypto');
const fs = require('fs');

const hash = crypto.createHash('sha256');

const input = fs.createReadStream(filename);
input.on('readable', () => {
  const data = input.read();
  if (data)
    hash.update(data);
  else {
    console.log(`${hash.digest('hex')} ${filename}`);
  }
});
```

### 对称加密解密 `cipher`
支持加密：aes192, ase256 

```
const crypto = require('crypto');

const cipher = crypto.createCipher('aes192', 'a password');
let encrypted = cipher.update('some clear text data', 'utf8', 'hex');
encrypted += cipher.final('hex');
console.log('加密：' + encrypted);
//加密：ca981be48e90867604588e75d04feabb63cc007a8f8ad89b10616ed84d815504

const decipher = crypto.createDecipher('aes192', 'a password');
let decrypted = decipher.update(encrypted, 'hex', 'utf8')
decrypted += decipher.final('utf8')
console.log('解密：' + decrypted);
//解密：some clear text data
```

## 设置模板全局常量
```
app.locals.blog = {
    title: pkg.name,
    description: pkg.description
};
```


### 添加模板必需的三个变量
```
app.use(function (req, res, next) {
    res.locals.user = req.session.user;
    res.locals.success = req.flash('success').toString();
    res.locals.error = req.flash('error').toString();
    next();
});
```


## cookie-parser
`cookie-parser` 在用 `express` 生成器构建项目时自动安装的，它的作用就是设置，获取和删除 `cookie`。`express-session` 依赖于它。

### 引入
```
var cookieParser = require('cookie-parser');    #引入模块
app.use(cookieParser());        #挂载中间件，可以理解为实例化

app.use(cookieParser('ruidoc')); # 签名需要传一个自定义字符串作为secret
```

### 创建cookie [api](http://www.expressjs.com.cn/4x/api.html#res.cookie)

```
res.cookie(name, value [, options]);
res.cookie('cart', { items: [1,2,3] });
res.cookie('cart', { items: [1,2,3] }, { maxAge: 900000 });
res.cookie('name', 'tobi', { domain: '.example.com', path: '/admin', secure: true });
```


### 获取cookie

```
var cookies = req.cookies      # 获取cookie集合
var value = req.cookies.key    # 获取名称为key的cookie的值
```

### 清楚
```
res.clearCookie(name [, options])
res.clearCookie('name', { path: '/admin' });
```
### 签名后获取 cookie
```
# 创建cookie的options中，必填 signed: true
res.cookie(name, value, {    
    'signed': true // 开启签名
});

var cookies = req.signedCookies      # 获取cookie集合
var value = req.signedCookies.key    # 获取名称为key的cookie的值
```


### 栗子
```
// 设置cookie名为user，值为对象，90000ms过期，无签名
res.cookie('user', {id: 1, name: 'ruidoc'}, {
        maxAge: 900000 
    }
);

//获取设置的cookie
var user = req.cookies.user

// 设置cookie名为admin，值为对象，无过期时间，有签名
res.cookie('admin', { 'id': 1 }, {
        'signed': true
    }
);
//获取设置的cookie
var admin = req.signedCookies.admin

```

## express-session
```
$ npm install express-session
var session = require('express-session')

app.use(session({
  secret: 'keyboard cat',
  resave: false,
  saveUninitialized: true,
  cookie: { secure: true }
}))

```
[node.js 中间件express-session使用详解](https://segmentfault.com/a/1190000010306099)

### 常用参数

`secret`:一个`String`类型的字符串，作为服务器端生成`session`的签名。 
`name`:返回客户端的key的名称，默认为connect.sid,也可以自己设置。 
`resave`:(是否允许)当客户端并行发送多个请求时，其中一个请求在另一个请求结束时对`session`进行修改覆盖并保存。
默认为`true`。但是(后续版本)有可能默认失效，所以最好手动添加。
`saveUninitialized`:初始化`session`时是否保存到存储。默认为`true`， 但是(后续版本)有可能默认失效，所以最好手动添加。
`cookie`:设置返回到前端key的属性，默认值为`{ path: ‘/', httpOnly: true, secure: false, maxAge: null }` 。

* `express-session`的一些方法:
* `Session.destroy()` :删除`session`，当检测到客户端关闭时调用。
* `Session.reload()` :当`session`有修改时，刷新`session`。
* `Session.regenerate()` ：将已有`session`初始化。
* `Session.save()` ：保存`session`。

### 例子
```
//app.js中添加如下代码(已有的不用添加)
var express = require('express');
var cookieParser = require('cookie-parser');
var session = require('express-session');
 
app.use(cookieParser('sessiontest'));
app.use(session({
 secret: 'sessiontest',//与cookieParser中的一致
 resave: true,
 saveUninitialized:true
}));
```

```
req.session.user=user;
req.session.destroy()      # 清空所有session
req.session.key.destroy()    # 销毁名称为key的session的值
```

### `var request = require('request')`
```
npm install --save request
npm install --save request-promise
```

```
const pixivision = (page) => {
    let options = {
        url: 'https://www.pixivision.net/zh/c/illustration?p=' + page,
        headers: {
            cookie: 'user_lang=zh'
        }
    }
    console.time(url);
    request(options, function (err, response, body) {
        if (!err && response.statusCode == 200) {
            console.timeEnd(url); // 通过time和timeEnd记录抓取url的时间
            var $ = cheerio.load(body);
            var data = [];
            $('.main-column-container .article-card-container').each(function () {
                var _this = $(this)
                var tags = []
                _this.find('.tls__list-item-container').each(function (index, el) {
                    var _tag = $(this).children('a')
                    tags.push({
                        tag: _tag.attr('data-gtm-label'),
                        id: _tag.attr('href').match(/\d+$/)[0]
                    })
                });
                data.push({
                    id: _this.find('.arc__title > a').attr('data-gtm-label'),
                    title: _this.find('.arc__title > a').text(),
                    tags: tags
                })
            })
            var next = $('a.next').attr('href').match(/\d+$/)[0]
            return {
                data: data,
                next: next
            }
        }
    });
}
```

### `var rp = require('request-promise');`

```
# https://github.com/request/request-promise
const pixivision = (page) => {
    let options = {
        uri: 'https://www.pixivision.net/zh/c/illustration?p=' + page,
        headers: {
            cookie: 'user_lang=zh'
        },
        transform: function (body) {
            return cheerio.load(body);
        }
    }
    rp(options).then(($) => {
        var data = []

        $('.main-column-container .article-card-container').each( (i, el) => {
            var _this = $(el);
            let tags = []
            _this.find('.tls__list-item-container').each(function (i, t) {
                let _t = $(t).children('a')
                tags.push({
                    tag: _t.attr('data-gtm-label'),
                    id: _t.attr('href').match(/\d+$/)[0]
                })
            });
            data.push({
                id: _this.find('.arc__title > a').attr('data-gtm-label'),
                title: _this.find('.arc__title > a').text(),
                tags: tags
            })
        })
        var next = $('a.next').attr('href').match(/\d+$/)[0]
        console.log({ data: data, next: next})
        res.send({ data: data, next: next})
    }).catch(err => {
        console.log(`error: ${err}`)
    })
}
```

## `Node.js`  `async.parallelLimit` 与 `async.eachLimit`
### `async.parallel`
多个异步并行执行，当所有异步函数执行完成后执行回调函数，回到函数的参数为之前异步函数执行结果的数组，如果需要限制并行执行的数量可以使用`parallelLimit`
```
async.parallel([
  function(callback) {
    doAsync1(arg, callback);
  },   
  function(callback) {
    doAsync2(arg, callback);
  },
  function(callback) {
    doAsync3(arg, callback);
  }
], function(err, result) {
  console.log(result);
});
```
在线demo: [http://jsbin.com/mugixu/edit?js,console,output](http://jsbin.com/mugixu/edit?js,console,output)

`async.parallelLimit` 方法接受两个参数，第一个参数为任务数组，每个任务是一个函数，第二个参数为每次并行执行的任务数，第三个参数为回调函数。使用 `async.parallelLimit` 完成发送邮件任务的思路是先使用数据与所要做的任务，组装成任务数组交给 `async.parallelLimit` 方法去执行。
```
let userEmailList = [ 'a@example.com', 'b@example.com', ..., 'z@example.com' ];
let limit = 5;
let taskList = userEmailList.map(function (email) {
    return function (callback) {
        sendEmail(email, function (error, result) {
            return callback(error, result);
        });
    }
});
async.parallel(taskList, limit, function (error, result) {
    console.log(error, result);
});
```

`async.eachLimit` 方法接受四个参数，第一个参数为原始数据数组，第二个参数为每次并行处理的数据量，第三个参数为 `方法` 需要为数据进行的函数，第四个参数为回调函数。使用 `async.eachLimit` 完成发送邮件任务的思路是定义一个对数据进行处理的函数，然后使用 `async.eachLimit` 将处理函数应用所有数据上。  
`async.each` 接受三个参数，比 `eachLimit` 少了第二个参数 `数量`。

```
let userEmailList = [ 'a@example.com', 'b@example.com', ..., 'z@example.com' ];
let limit = 5;
let processer = function (email) {
    sendEmail(email, function (error) {
        return callback(error, result);
    });
}
async.eachLimit(userEmailList, limit, processer, function (error, result){
    console.log(error);
});
```

### `async.series`
多个异步依次顺序执行。如果有异常抛出时就立即执行回调函数， 回调函数的`err`为抛出的异常；如果没有异常抛出，当所有异步函数完成后，执行成功回调函数，`err`值为`null`, `result` 为异步数组的结果数组
```
async.series([
  function(callback) {
    doAsync1(arg, callback);
  },   
  function(callback) {
    doAsync2(arg, callback);
  },
  function(callback) {
    doAsync3(arg, callback);
  }
], function(err, result) {
  console.log(result);
});
```
在线demo: [http://jsbin.com/sisasu/edit?js,console,output](http://jsbin.com/sisasu/edit?js,console,output)

### `async.waterfall`
多个异步依次顺序执行，且后面异步函数的依赖前面异步函数的输出
```
async.waterfall([
  function(callback) {
    doAsync1(2, callback);
  },
  function(arg, callback) {
    doAsync2(arg, callback);
  },
  function(arg, callback) {
    doAsync3(arg, callback);
  }
], function(err, result) {
  console.log(result);
});
```
在线demo: [http://jsbin.com/yozuje/edit?js,console,output](http://jsbin.com/yozuje/edit?js,console,output)

## `whilst(test, iteratee, callback)`
Repeatedly call `iteratee`, while `test` returns `true`. Calls `callback` when stopped, or an error occurs.
[https://caolan.github.io/async/docs.html#whilst](https://caolan.github.io/async/docs.html#whilst)

## `doWhilst(iteratee, test, callback)`
同上，和`do while` 特性一样，先执行一次，再判断 `test` 


## `redis`

* https://www.npmjs.com/package/redis

* https://github.com/NodeRedis/node_redis

* [Redis使用与实践](https://segmentfault.com/a/1190000010675699)


```
var redis  = require("redis"),
    client = redis.createClient(), multi;
```
### FLUSHALL 
删除全部

`clinet.multi`

`clinet.end`

`clinet.rpush`

`clinet.decr`

`clinet.get`

`clinet.set`

`client.lpop`

`incr `

`decr`


## AXIOS
```
axios.request(config)

axios.get(url[, config])

axios.delete(url[, config])

axios.head(url[, config])

axios.options(url[, config])

axios.post(url[, data[, config]])

axios.put(url[, data[, config]])

axios.patch(url[, data[, config]])
```