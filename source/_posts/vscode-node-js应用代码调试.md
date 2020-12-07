---
title: vscode node.js应用代码调试
date: 2020-04-12 20:14:14
tags: 前端
categories: javascript
---

- vscode代码配置:

  ```javascript
  {
      // 使用 IntelliSense 了解相关属性。 
      // 悬停以查看现有属性的描述。
      // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
      "version": "0.2.0",
      "configurations": [{
          "type": "node",
          "request": "launch",
          "name": "启动程序",
          // "program": "${workspaceRoot}/node/index.js"
          "runtimeExecutable": "nodemon",
          "args": ["${workspaceRoot}/node/index.js"],
          "restart": true,
          "protocol": "inspector",
          "sourceMaps": true,
          "console": "integratedTerminal",
          "internalConsoleOptions": "neverOpen",
          // 其他附加配置
          "runtimeArgs": []
      }]
  }
  ```

- 参考: <a href="https://juejin.im/post/5b270887e51d45589175afc3">地址</a>
