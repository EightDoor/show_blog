---
title: angular打包错误ERROR in Illegal State issue when giving angular build
date: 2020-02-19 11:17:13
tags: 工具
categories: 打包
---

- 执行 ng build --prod

  - 错误描述:

    ```powershell
    ERROR in Illegal State: referring to a type without a variable {"filePath":"/home/jenkins/agent/workspace/smartcampus/frontend/smartcampus-school-pc-cn-develop/node_modules/ng-zorro-antd/table/ng-zorro-antd-table.d.ts","name":"NzTrDirective","members":[]}
    ```

  - 解决方案: 文件

    ```javascript
    tsconfig.json -> fullTemplateTypeCheck->false
    ```

  - 继续执行 ng build --prod 根据提示修改对应错误
