---
type: nodejs
---
## 进程 `process`

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
    res.send(files) //["daily-20170417-1.json","daily-20170418-1.json","female-20170424-1.json"]
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
