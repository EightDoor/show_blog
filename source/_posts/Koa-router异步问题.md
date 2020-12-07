---
title: Koa-router异步问题
date: 2020-03-12 11:03:40
tags: koa
categories: 前端
---

# Koa-router 请求异步问题

- 参考: <a href="https://blog.csdn.net/strangedbly/article/details/61925167">地址</a>

- koa-router源码: <a href="https://github.com/alexmingoia/koa-router/blob/master/test/lib/router.js">地址</a>

- 通过promise实现

  ```javascript
  router.get('/double', function(ctx, next) {
        return new Promise(function(resolve, reject) {//关键啊，文档中居然没有
          setTimeout(function() {
            ctx.body = {message: 'Hello'};         //这就是我遇到的问题啊。异步中的ctx.body赋值。
            resolve(next());
          }, 1);
        });
      }, function(ctx, next) {
        return new Promise(function(resolve, reject) {
          setTimeout(function() {
            ctx.body.message += ' World';
            resolve(next());
          }, 1);
        });
      }, function(ctx, next) {
        ctx.body.message += '!';
      });
  ```

  
