---
title: ionic请求jira接口
date: 2020-01-12 15:00:00
tags:
---

# 记录

- 背景:  jira项目   手机端app展示、统计项目信息。接口请求采用cookie、session认证方式

- 手机端: 采用angular+ionic 搭建app、接口请求采用httpClient。

- 遇到的问题:  手机端访问时候 跨域, cookie无法携带到请求头。

  - 解决方案:

     1.登录成功存储本地cookie(必须设置路径为/)

     ```javascript
    // 设置登录的cooike
    document.cookie = `JSESSIONID=25911C87CD11A87ADE4874F11679A2E4;path=/`;
     ```

    2.请求接口时设置

    ```javascript
    const headers = [{ 'Content-Type': 'application/json' }]
    const headerOptions = {headers,withCredentials:true}
    ```

    参考链接: 

    <a href="https://blog.csdn.net/mengzuchao/article/details/84324713" target="_blank">js cookie 的路径以及 Cookie 域</a>

    <a href="https://blog.csdn.net/weboof/article/details/79799231" target="_blank">cookie跨域问题</a>

    

