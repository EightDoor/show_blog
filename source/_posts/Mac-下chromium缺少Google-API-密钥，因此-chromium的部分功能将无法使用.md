---
title: Mac 下chromium缺少Google API 密钥，因此 chromium的部分功能将无法使用
date: 2020-02-09 15:09:25
tags: 工具
categories: 其他
---

chromium 使用登录功能需要配置 GoogleAPI 密钥，相关获取方式可以自动搜索

本文主要讲述 Mac 下配置

如果缺失无法登陆谷歌账号，Chrome 的书签同步功能无法使用；

解决方案：

第一步

```shell
mv /Applications/Chromium.app/Contents/MacOS/Chromium /Applications/Chromium.app/Contents/MacOS/Chromium_bin
```

第二步
vi /Applications/Chromium.app/Contents/MacOS/Chromium

```shell
#!/bin/bash

# Set up environment variables
export GOOGLE_API_KEY="xxxxxxx"
export GOOGLE_DEFAULT_CLIENT_ID="xxxxxxx"
export GOOGLE_DEFAULT_CLIENT_SECRET="xxxxxx"

# Launch Chromium
/Applications/Chromium.app/Contents/MacOS/Chromium_bin
```

第三步
chmod +x /Applications/Chromium.app/Contents/MacOS/Chromium

完成

参考文档：https://gist.github.com/cvan/44a6d60457b20133191bd7b104f9dcc4
