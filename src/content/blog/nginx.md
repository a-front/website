---
title: 'micro-app 对接文档'
description: 'nginx 配置'
pubDate: '2023-11-22'
---

##### nginx 配置如下;

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


<!-- 详细可参考 [MicroApp文档](https://zeroing.jd.com/docs.html#/zh-cn/framework/vue); -->

<!-- 主要有3个点需要注意: 
1. 路由basepath;
2. 相对路径 需配置动态 publicPath;
3. 跨域问题 参考nginx 配置;
 -->


