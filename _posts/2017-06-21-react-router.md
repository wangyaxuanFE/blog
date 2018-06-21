---
layout: page
title:  "react-router 与 react-router-dom 的区别"
teaser: "所用所想"
categories:
    - fe
tags:
    - http
image:
#   thumb: "unsplash_brooklyn-bridge-thumb.jpg"
header: no
---

## react-router-dom的产生

  React Router 4.0 采用单代码仓库模型架构（monorepo），这意味者这个仓库里面有若干相互独立的包，分别是：
  * react-router React Router 核心
  * react-router-dom 用于 DOM 绑定的 React Router
  * react-router-native 用于 React Native 的 React Router
  * react-router-redux React Router 和 Redux 的集成
  * react-router-config 静态路由配置的小助手


## react-router 还是 react-router-dom？

  * react-router-dom中的Route.js和Router.js，都是直接导入的react-router中的Route.js和Router.js。

```js
"use strict";

exports.__esModule = true;

var _Route = require("react-router/Route");

var _Route2 = _interopRequireDefault(_Route);

function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }

exports.default = _Route2.default; // Written in this round about way for babel-transform-imports
```

  * react-router提供的是路由的基本功能，react-router-dom根据在浏览器运行时路由的特征，在react-router
之上做了一层封装，提供了HashRouter、BrowserRouter等在web端常用的路由。
  * 如果是在web端使用的话，package.json中直接引入react-router-dom就可以。
