---
title: Vue3.0响应式原理
date: 2020-01-22 20:48:52
tags: 前端
categories: vue
---

- 实现方法:

  ```javascript
  // Vue3.0 响应式原理
  // 1). 2.0默认会递归(数据尽量扁平化) 2). 2.0数组改变length是无效的 3). 2.0对象不存在的属性不能被拦截

  // proxy 兼容性差 ie11不兼容

  let toProxy = new WeakMap(); // 弱引用映射表 es6 放置的是 原对象:代理过的对象
  let toRaw = new WeakMap(); // 被代理过得对象:原对象
  // 判断是不是对象
  function isObject(val) {
    return typeof val === "object" && val !== null;
  }

  // 判断对象有没有属性
  function hasOwn(target, key) {
    return target.hasOwnProperty(key);
  }
  // 响应式的核心方法
  function reactive(target) {
    // 创建响应式对象
    return createReactiveObject(target);
  }

  // 创建响应式对象
  function createReactiveObject(target) {
    if (!isObject(target)) {
      // 如果当前不是对象， 直接返回
      return target;
    }
    // 如果已经代理过了，就将代理过得结果返回;
    let proxy = toProxy.get(target);
    if (proxy) {
      return proxy;
    }
    // 防止代理过得对象再次被代理
    if (toRaw.has(target)) {
      return target;
    }
    let baseHandler = {
      // reflect 优点: 不会报错，而且会有返回值，会替代掉Object上的方法

      // target - 源对象, key - 键值, receiver - 新的proxy代理对象
      get(target, key, receiver) {
        // 收集依赖 订阅 把当前的key和这个effect对应起来
        track(target, key); // 如果目标上的 这个key变化  从新让数组中的effect执行即可

        // console.log('获取');
        // proxy + reflect 反射
        let result = Reflect.get(target, key, receiver);
        // result 是当前获取到的值
        return isObject(result) ? reactive(result) : result; // 是个递归
        // return target[key];
      },
      set(target, key, value, receiver) {
        // 怎么去 识别改属性 还是新增属性
        let hasKey = hasOwn(target, key); // 判断这个属性 以前有没有
        // console.log(key, value);
        // console.log('设置');
        let oldValue = target[key];
        let res = Reflect.set(target, key, value, receiver); // 返回boolean值
        if (!hasKey) {
          trigger(target, "add", key);
          console.log("新增属性");
        } else if (oldValue !== value) {
          // 表示属性更改了
          trigger(target, "set", key);
          console.log("修改属性");
        } // 为了屏蔽无意义的修改
        return res;
        // target[key] = value; //缺点 如果设置没成功 如果这个对象不可以被更改(writable) - 没有返回值
      },
      deleteProperty(target, key) {
        let res = Reflect.deleteProperty(target, key);
        console.log("删除");
        return res;
      }
    };

    let observed = new Proxy(target, baseHandler); // es6
    // 需要记录一下 如果这个对象代理过了 就不要再new了
    // hash表 映射表 {key=>value}
    toProxy.set(target, observed);
    toRaw.set(observed, target);
    return observed;
  }

  // let obj = {
  //   name: 'zk'
  // }

  // 代理对象
  let proxy = reactive({
    name: "zk",

    // 多层代理 通过get方法来判断
    info: {
      n: 10
    }
  });

  // reactive(proxy);
  // reactive(obj);

  // proxy.name = '改变的值zk';
  // proxy.info.n = 300
  // console.log(proxy.name);
  // console.log(proxy.info.n);
  // delete proxy.name;
  // console.log(proxy.name);

  // proxy.a
  // proxy.name = '改变的值';
  // delete proxy.name;

  // let arr = [1, 2, 3]
  // let proxyArr = reactive(arr)
  // proxyArr.push(4)
  // proxyArr.length = 4;

  // 栈 先进后出{name:[effect]}
  let activeEffectStacks = []; // 栈型结果

  // 数据结构
  // {
  //   target: {
  //     key: [fn, fn]
  //   }
  // }

  let targetsMap = new WeakMap(); // 集合和hash表

  function track(target, key) {
    // 如果这个target中的key变化了 我就执行数组里的方法
    let effect = activeEffectStacks[activeEffectStacks.length - 1];
    if (effect) {
      // 有对应关系 才创建关联
      let depsMap = targetsMap.get(target);
      if (!depsMap) {
        targetsMap.set(target, (depsMap = new Map()));
      }
      let deps = depsMap.get(key);
      if (!deps) {
        depsMap.set(key, (deps = new Set()));
      }
      if (!deps.has(effect)) {
        deps.add(effect);
      }
      // 动态创建依赖关系
    }
    // 什么都不做
  }

  function trigger(target, type, key) {
    let depsMap = targetsMap.get(target);
    if (depsMap) {
      let deps = depsMap.get(key);
      if (deps) {
        // 将当前key对应的effect依次执行
        deps.forEach(effect => {
          effect();
        });
      }
    }
  }

  // 响应式副作用
  function effect(fn) {
    // 需要把fn这个函数变成响应式的函数
    let effect = createReactiveEffect(fn);
    effect(); // 默认 应该先执行一次
  }

  function createReactiveEffect(fn) {
    let effect = function() {
      // 这个就是创建的响应式的effect
      return run(effect, fn); // 1.让fn执行 2.把这个effect存到栈中
    };
    return effect;
  }

  function run(effect, fn) {
    // 运行fn, 并且将effect存起来
    try {
      activeEffectStacks.push(effect);
      fn(); // vue2 利用了js是单线程的
    } finally {
      activeEffectStacks.pop();
    }
  }

  // 依赖收集 发布订阅
  let obj = reactive({
    name: "zk"
  });
  effect(() => {
    // effect 会执行两次 默认先执行一次 之后依赖的数据发生变化了 会再次执行
    console.log(obj.name, "effect"); // 会调用get方法
  });
  obj.name = "更改哦1";
  ```

- 参考资料: <a href="https://www.bilibili.com/video/av71321311?p=2">Vue3.0 响应式原理第二节</a>
- 参考资料: <a href="https://www.bilibili.com/video/av38284793/?spm_id_from=333.788.videocard.3">Vue 作者尤雨溪为你分享：Vue 3.0 进展@VueConf CN 2018</a>
- 参考资料: <a href="https://www.bilibili.com/video/av51444410/?spm_id_from=333.788.videocard.6">尤雨溪教你写 vue 高级 vue 教程 源码分析</a>
