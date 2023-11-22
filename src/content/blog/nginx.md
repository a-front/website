---
title: 'micro-app 对接文档'
description: 'nginx 配置'
pubDate: '2023-11-22'
---

nginx 配置如下;

``` nginx
add_header 'Access-Control-Allow-Origin' '*' allways;
add_header 'Access-Control-Allow-Methods' 'GET,POST,OPTIONS' allways;
# 根据需要添加header
add_header 'Access-Control-Allow-Headers' 'X-Origin,X-User-Id,SCM-Authorization,Authorization' allways;


location / {
  if($request_method='OPTIONS') {
    return 204
  }
}

location ^~ /api/ {
  if($request_method='OPTIONS') {
    return 204
  }
}
```
