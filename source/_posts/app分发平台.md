---
title: app分发平台
date: 2020-01-11 23:13:57
tags:
---

# 记录

- 为方便公司内部 app 版本统一管理,根据开源项目本地部署: <a href="https://github.com/rock-app/fabu.love">开源地址</a>

- 采坑记录

  1.安装依赖报错

  ```
  internal/util/inspect.js:31

  const types = internalBinding('types');

  ReferenceError: internalBinding is not defined
  ```

  错误原因：这个问题是我将 node 版本升级至 v10.15.0，npm 升级至 6.4.1 后出现的，在此之前，我的 node 版本是 8+，没有出现这个问题。

  - 解决方案: 升级下 native 这个插件的版本即可

  ​ `npm install natives@1.1.6`

  - 错误原地址:<a href="http://www.jsphp.net/gulp/show-27-98-1.html">地址</a>

    2.pm2 启动后台服务报错

- 解决方案: 在 server 根目录新增`app.js`，添加如下内容

  ```javascript
  require("babel-register");
  require("babel-polyfill");
  require("./index.js");
  ```

  然后再执行`pm2 start app.js`即可

- 解决方案原地址:<a href="https://github.com/rock-app/fabu.love/issues/90">地址</a>

  3.nginx 配置 upload 文件下载 404

- 正确配置:

  ```nginx
   location / {
          try_files $uri $uri/ @router;
          index index.html;
          root /www/wwwroot/app.start6.cn/dist;
      }

      location /upload {
          #该root目录为根目录下config.json文件里dir目录 上传的apk和ipa文件当作静态文件处理
          alias /apk/upload;

      }

      location @router {  # vue的router配置
          rewrite ^.*$ /index.html last;
      }

      location /api/ {  #把以api打头的接口转发给后端server
        proxy_pass http://127.0.0.1:9898; #这里端口修改为后端服务运行的端口
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      }
      client_max_body_size 208M; #最大上传的ipa/apk文件大小
  ```
