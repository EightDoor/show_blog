---
title: 下载electron失败
date: 2020-02-22 14:51:03
tags: 前端
categories: electron
---

- 使用`npm install electron --verbose`查看具体报错原因

- mac解决方案:

  - 进入~/.npmrc

  - 解决方案一:  taobao镜像下载增加如下内容

    ```shell
    registry=https://registry.npm.taobao.org
    sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
    phantomjs_cdnurl=http://npm.taobao.org/mirrors/phantomjs
    electron_mirror=http://npm.taobao.org/mirrors/electron/
    ```

  - 方案二:

    - 进到项目目录 node_modules\electron\install.js 找到如下代码并且修改

      ```javascript
      // downloads if not cached
      downloadArtifact({
        version,
        artifactName: 'electron',
        force: process.env.force_no_cache === 'true',
        cacheRoot: process.env.electron_config_cache,
        platform: process.env.npm_config_platform || process.platform,
        arch: process.env.npm_config_arch || process.arch,
        //添加如下代码，
        mirrorOptions:{
          mirror: 'https://npm.taobao.org/mirrors/electron/',
          customDir: version
        }
      }).then((zipPath) => extractFile(zipPath)).catch((err) => onerror(err))
      ```

    - 再次用终端打开 项目目录下的 `node_modules\electron` 运行 `node install.js`

    - 完美解决

