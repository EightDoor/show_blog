---
title: 【ES6基础】Map与WeakMap
date: 2020-01-22 11:20:04
tags: 前端
categories: javascript
---

## Map 常用方法示例

|      操作方法      |                   内容描述                   |
| :----------------: | :------------------------------------------: |
| map.set(key,value) |              添加键值对到映射中              |
|    map.get(key)    |          获取映射中某一个键的对应值          |
|  map.delete(key)   |             将某一键值对移除映射             |
|    Map.clear()     |             清空映射中所有键值对             |
|   map.entries()    |  返回一个以二元数组（键值对）作为元素的数组  |
|    map.has(key)    |         检查映射中是否包含某一键值对         |
|     map.keys()     | 返回一个当前映射中所有键作为元素的可迭代对象 |
|    map.values()    | 返回一个当前映射中所有值作为元素的可迭代对象 |
|      map.size      |              映射中键值对的数量              |

## Map 与 Object 的区别

| 对比项                       | 映射对象 Map | Object 对象 |
| :--------------------------- | :----------- | :---------- |
| 存储键值对                   | √            | √           |
| 遍历所有的键值对             | √            | √           |
| 检查是否包含指定的键值对     | √            | √           |
| 使用字符串作为键             | √            | √           |
| 使用 Symbol 作为键           | √            | √           |
| 使用任意对象作为键           | √            |             |
| 可以很方便的得知键值对的数量 | √            |             |

## WeakMap

- 与集合类型（Set）一样，映射类型也有一个 Weak 版本的 WeakMap。WeakMap 和 WeakSet 很相似，只不过 WeakMap 的键会检查变量的引用，只要其中任意一个引用被释放，该键值对就会被删除。

- 以下三点是 Map 和 WeakMap 的主要区别：

  - Map 对象的键可以是任何类型，但 WeakMap 对象中的键只能是对象引用
  - WeakMap 不能包含无引用的对象，否则会被自动清除出集合（垃圾回收机制)
  - WeakSet 对象是不可枚举的，无法获取大小

- 下段代码示例验证了 WeakMap 的以上特性：

  ```javascript
  let weakmap = new WeakMap();
  (function() {
    let o = { n: 1 };
    weakmap.set(o, "A");
  })(); // here 'o' key is garbage collected
  let s = { m: 1 };
  weakmap.set(s, "B");
  console.log(weakmap.get(s));
  console.log(...weakmap); // exception thrown
  weakmap.delete(s);
  weakmap.clear(); // Exception, no such function
  let weakmap_1 = new WeakMap([
    [{}, 2],
    [{}, 5]
  ]); //this works
  console.log(weakmap_1.size); //undefined”

  // --------------分割线--------------
  const weakmap = new WeakMap();
  let keyObject = { id: 1 };
  const valObject = { score: 100 };
  weakmap.set(keyObject, valObject);
  console.log(weakmap.get(keyObject));
  //output { score: 100 }
  keyObject = null;
  console.log(weakmap.has(keyObject));
  //output false
  ```

## 小结

今天的内容就介绍到这里，我们明白了 Map 是一个键值对的映射对象，相比 Object 来说可以使用任何键做为键值，并且能够很方便的获取键值对。WeakMap 相对于 Map 是一个不可枚举的对象，必须使用对象作为键值。如何更好的使用 Map 和 WeakMap 还需要具体结合我们实际的业务场景进行灵活使用。

参考资料: <a href="https://blog.csdn.net/Ed7zgeE9X/article/details/89879406">Map 与 WeakMap</a>
