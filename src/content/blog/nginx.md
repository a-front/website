---
title: 'micro-app 对接文档'
description: 'nginx 配置'
pubDate: '2023-11-22'
---

##### nginx 配置如下;

``` nginx
add_header 'Access-Control-Allow-Origin' '*' always;
add_header 'Access-Control-Allow-Methods' 'GET,POST,OPTIONS' always;
# 根据需要添加header
add_header 'Access-Control-Allow-Headers' 'X-Origin,X-User-Id,SCM-Authorization,Authorization' always;


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

1. 如果使用了 


详细可参考 [MicroApp文档](https://zeroing.jd.com/docs.html#/zh-cn/framework/vue);

主要有3个点需要注意: 
1. 路由basepath;
2. 相对路径 需配置动态 publicPath;
3. 跨域问题 参考nginx 配置;



