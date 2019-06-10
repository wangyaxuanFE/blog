---
layout: post
title: react-router-redux还需要用吗
date: 2017-09-12 13:32:20 +0300
# description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: 6.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Holidays, Hawaii]
---
## react-router-redux是什么

  是将react-router 和 redux 集成到一起的库，让你可以用redux的方式去操作react-router。
  本质上，是把react-router自己维护的状态，例如location、history、path等等，也交给redux管理 。


## 以前的用法
----
  **routerActions**

  实现了组件内部跳转～

```js
import {routerActions} from 'react-router-redux'
import { bindActionCreators } from 'redux'
function mapDispatchToProps(dispatch) {
  return {
    actions: bindActionCreators(Actions, dispatch),
    routerActions: bindActionCreators(routerActions, dispatch)
  }
}

routerActions.push('/user');
routerActions.replace('user/validatesuccess');
```

  **routerReducer , routerMiddleware**

    * store传入路由信息～
    * 中间件传入，可以直接从state 中的router属性中获取location，history等对象～

```js
const store = createStore(
    combineReducers({
        ...reducer,
        router: routerReducer
    }),
    applyMiddleware(routeMiddleware));
```

  **ConnectedRouter**

```js
import { ConnectedRouter,syncHistoryWithStore } from 'react-router-redux'
import { browserHistory } from 'react-router'
const history = syncHistoryWithStore(browserHistory, store);
const app = (
    <Provider store={store}>
      <ConnectedRouter history={history}>
        <TheApp/>
      </ConnectedRouter>
    </Provider>
  );
```



## react-router本身也提供了解决上述问题的方法
-----

```js
//在组件外部使用导航
import { browserHistory } from 'react-router'
browserHistory.push('/some/path')

//组件内部可跳转

//1，react-router v4 在 Router 组件中通过Contex暴露了一个router对象~
//在子组件中使用Context，我们可以获得router对象
this.context.router.history.push("/some/Path");

//2，使用 withRouter
//withRouter可以包装任何自定义组件，将react-router 的 history,location,match 三个对象传入,
//获取路由信息
import {withRouter} from "react-router-dom";
class MyComponent extends React.Component {
  ...
  myFunction() {
    this.props.history.push("/some/Path");
  }
  ...
}
export default withRouter(MyComponent);
```

**总结：一般情况下，是没有必要使用这个库的 ～**
