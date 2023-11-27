---
title: 'micro-app 对接文档'
description: 'nginx 配置'
pubDate: '2023-11-22'
---

##### nginx 配置如下;
nginx 配置主要解决跨域问题, 可以参考下列配置

``` nginx
add_header 'Access-Control-Allow-Origin' $http_origin always;
add_header 'Access-Control-Allow-Methods' '*' always;
# 根据需要添加header
add_header 'Access-Control-Allow-Headers' '*' always;

proxy_hide_header Access-Control-Allow-Origin;
proxy_hide_header Access-Control-Request-Method;
proxy_hide_header Access-Control-Request-Headers;
proxy_hide_header Access-Control-Allow-Credentials;
proxy_hide_header Vary;

location / {
  if ($request_method = 'OPTIONS') {
    return 204;
  }
}

location ^~ /api/ {
  if ($request_method = 'OPTIONS') {
    return 204;
  }
}
```

##### 代码层面对接

1. 在子应用中通过 window.__MICRO_APP_ENVIRONMENT__ 判断是否在微前端环境中。
```js
if (window.__MICRO_APP_ENVIRONMENT__) {
  console.log('我在微前端环境中')
}
```

2. 使用了`浏览器路由` 请用 window.__MICRO_APP_BASE_ROUTE__ 设置basename

react
```js

import { BrowserRouter, Switch, Route } from 'react-router-dom'

export default function AppRoute () {
  return (
    // 👇 设置基础路由，子应用可以通过window.__MICRO_APP_BASE_ROUTE__获取主应用下发的baseroute，如果没有设置baseroute属性，则此值默认为空字符串
    <BrowserRouter basename={window.__MICRO_APP_BASE_ROUTE__ || '/'}>
      ...
    </BrowserRouter>
  )
}

```

vue
```js 
import Vue from 'vue'
import VueRouter from 'vue-router'
import routes from './router'

const router = new VueRouter({
  // 👇 设置基础路由，子应用可以通过window.__MICRO_APP_BASE_ROUTE__获取主应用下发的baseroute，如果没有设置baseroute属性，则此值默认为空字符串
  base: window.__MICRO_APP_BASE_ROUTE__ || '/',
  routes,
})

let app = new Vue({
  router,
  render: h => h(App),
}).$mount('#app')

```

3. 如何项目中使用了相对路径, 请配置动态public path
  
``` js
  if (window.__MICRO_APP_ENVIRONMENT__) {
    // eslint-disable-next-line
    __webpack_public_path__ = window.__MICRO_APP_PUBLIC_PATH__
  }
```

详细可参考 [MicroApp文档](https://zeroing.jd.com/docs.html#/zh-cn/framework/vue);

<!-- 主要有3个点需要注意: 
1. 路由basepath;
2. 相对路径 需配置动态 publicPath;
3. 跨域问题 参考nginx 配置;
 -->


