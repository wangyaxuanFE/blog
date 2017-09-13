---
layout: page
title:  "Cookie简介"
subheadline:  "一篇简单的cookie介绍"
teaser: "cookie是一种本地存储，用于区分客户端，辨别用户状态，在HTTP中广泛使用。作为前端工程师一定要了解的一个方面知识，cookie在前端知识点中也占据了非常重要的地位。"
categories:
    - fe
tags:
    - http
image:
#   thumb: "unsplash_brooklyn-bridge-thumb.jpg"
header: no
---

#### 一个例子

用js设置一个cookie

```js
document.cookie = 'name=value; domain=.afb.com; expires=Mon, 24 Jul 2018 09:36:46 GMT; path=/; secure'
```
这里进行了一个cookie设置操作，参数有`name`, `domain`, `expries`, `path`, `secure`这些。参数之间用分号`;`链接


#### 参数

- ##### path

  - path默认值为当前文档位置的路径
  - 可以设置成根目录'/'，所有的子目录都可以访问

- ##### domain

  - domain默认为当前文档位置的路径的域名部分
  - 可以设置成'.domain.com'，主域名和所有的子域名(m.news.domain.com,m.domain.com)都可以访问到
  - 如果设置成'.news.domain.com'，其子域名(m.news.domain.com)可以访问到
  - 可以设置成'.domain.com'，主域名和所有的子域名(www.domain.com,m.domain.com)都可以访问到
  - 尽量不要用'domain.com'这样的值

- ##### max-age 和 expires

  - max-age 单位秒，默认为-1，值为过期时长 (例如一年为60*60*24*365)
  - max-age IE低版本不支持，统一换算成 expires (当前时间+max-age)
  - expires 不指定时，会话结束后过期
  - expires 格式应为UTC时间，这个值的格式参见Date.toUTCString()

- ##### secure

  - secure (cookie只通过https协议传输)

- ##### httpOnly

  - httpOnly (服务端用，前端无法取到)

- ##### name=value

  - name 本条cookie的键名
  - value 可以用encodeURIComponent()来保证它不包含任何逗号、分号或空格(cookie值中禁止使用这些值).

#### 传输与解析

HTTP请求时浏览器自动通过Request的headers.Cookie传输给服务器，服务器通过Response的headers里的多条Set-cookie存储到浏览器

![](/images/6553C236-184C-4A8B-ACCF-397B14764916.png)
Response

![](/images/113CE94D-266E-481A-8AA8-7D0B2CCFC3D9.png)
Request

如果符合当前域名和路径下，不同domain不同path下存在相同name的cookie，都会传输，传输顺序按照设置的时间正序排列。

```
Cookie: name=value; name2=value2
```

解析时，如果存在相同name的值，以第一个的值为最终结果。

如果接收到的cookie是`name=value; name=value2`，解析的结果是

```js
cookies = { name:'value' }
```

#### 相关类库

##### nodejs

express 官方 [cookie-parser](https://www.npmjs.com/package/cookie-parser)

```js
var express      = require('express')
var cookieParser = require('cookie-parser')

var app = express()
app.use(cookieParser())

app.get('/', function(req, res) {
  // get
  console.log('Cookies: ', req.cookies)
  console.log('name', req.cookies.name)
  // set
  res.cookie('name', 'tobi', { domain: '.example.com', path: '/admin', secure: true })
  res.cookie('rememberme', '1', { expires: new Date(Date.now() + 900000), httpOnly: true })
  // clear
  res.clearCookie('name', { path: '/admin' });
})

app.listen(8080)
```

koa 依赖 [cookies](https://www.npmjs.com/package/cookies)

```js
app.use(async (ctx, next) => {
  // get
  console.log('name', ctx.cookies.get('name'))
  // set
  ctx.cookies.set('name', 'tobi', { domain: '.example.com', path: '/admin', secure: true })
});
```

##### 浏览器

[js-cookie](https://www.npmjs.com/package/js-cookie)

```js
// Create a cookie, valid across the entire site:
Cookies.set('name', 'value');

// Create a cookie that expires 7 days from now, valid across the entire site:
Cookies.set('name', 'value', { expires: 7 });

// Create an expiring cookie, valid to the path of the current page:
Cookies.set('name', 'value', { expires: 7, path: '' });

// Read cookie:
Cookies.get('name'); // => 'value'
Cookies.get('nothing'); // => undefined

// Read all visible cookies:
Cookies.get(); // => { name: 'value' }

// Delete cookie:
Cookies.remove('name');

// Delete a cookie valid to the path of the current page:
Cookies.set('name', 'value', { path: '' });
Cookies.remove('name'); // fail!
Cookies.remove('name', { path: '' }); // removed!
```

#### 相关规范

[rfc6265](https://tools.ietf.org/html/rfc6265)

### 所有`http`相关文章
{: .t60 }

{% include list-posts tag='http' %}

