---
title: "vue2.0响应式原理实现(对象,数组)"
date: 2020-01-22 12:37:10
tags: 前端
categories: vue
---

# Vue2.0 响应式原理实现(对象，数组)

- 具体实现:

  ```javascript
  // Vue2.0如何实现响应式原理

  // 拿到原来原型上的方法
  let oldArrayPrototype = Array.prototype;
  // 创建新的实例，获取原型所有方法，以免影响原型方法。
  let propto = Object.create(oldArrayPrototype); // 继承
  ["push", "shift", "unshift"].forEach(method => {
    propto[method] = function() {
      // 函数劫持, 把函数进行重写 内部调用原来的方法
      // call参考地址: https://blog.csdn.net/mandyucan/article/details/80820139
      updateView(); // 切片编程
      oldArrayPrototype[method].call(this, ...arguments);
    };
  });

  function observer(target) {
    // 判断是否是对象
    if (typeof target !== "object" || target === null) {
      return target;
    }

    // 判断是否是数组
    if (Array.isArray(target)) {
      // 拦截数组 给数组的方法进行重写
      // 兼容写法 https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/setPrototypeOf
      Object.setPrototypeOf(target, propto);
      // target.__proto__ = propto
    }

    // 重新定义属性，方法
    for (let key in target) {
      defineReactive(target, key, target[key]);
    }
  }

  function defineReactive(target, key, value) {
    // 值的类型可能为对象属性，继续拦截,处理
    observer(value);
    Object.defineProperty(target, key, {
      get() {
        // get进行依赖收集
        return value;
      },
      set(newValue) {
        if (newValue !== value) {
          // 从新定义值可能为对象属性，处理
          observer(value);

          updateView();
          value = newValue;
        }
      }
    });
  }

  // 数据变化可以更新视图
  function updateView() {
    console.log("更新视图");
  }

  // 使用Object.defineProperty 就是可以重新定义属性,给属性增加getter和setter
  // 问题1: 对象层级嵌套太深 递归影响性能
  // 问题2: 如果属性不存在，新增的属性不会是响应式的

  // 对象
  let data = {
    name: "zf",
    age: {
      n: 200
    }
  };

  // 观察数据
  observer(data);

  data.name = "jw";
  data.age.n = 300;

  // data.age = {
  //   n: 100
  // };
  // data.age.n = 400;
  console.log(data.name);
  console.log(data.age.n);

  // 数组
  let array = {
    name: "zk",
    age: [1, 2, 3]
  };

  observer(array);
  array.age.push(4); // 需要对数组上的方法进行重写 push、shift、unshift、pop、reverse
  console.log(array.age);
  ```

- 参考地址: <a href="https://www.bilibili.com/video/av71321311/">vue3.0 源码实现原理第一节</a>
