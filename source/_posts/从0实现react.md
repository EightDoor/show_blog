---
title: 从0实现react
date: 2020-01-23 21:47:03
tags: 前端
categories: react
---

项目地址: <a href="https://gitee.com/EightDoor/react-simple" target="_blank">项目地地址</a>
参考地址: <a href="https://www.bilibili.com/video/av74527572?from=search&seid=16681124829616503908">bilibili</a>

```
1.下载nodejs
2.下载脚手架: npm install create-react-app -g
3.创建项目:create-react-app react-test
```

1.火热的0配置的打包工具parcel

- 地址: <a href="https://parceljs.org/getting_started.html">parcel官网</a>

2.安装babel插件,将jsx语法转换成js对象(虚拟DOM)

- ```javascript
  npm install babel-core babel-preset-env babel-plugin-transform-react-jsx --save-dev
  ```

- <a href="https://www.babeljs.cn/">babel官网</a>

3.组件和生命周期

4.diff算法

### diff算法(待完成)

- 如何减少DOM更新: 我们需要找出渲染前后真正变化的部分,只更新这一部分,而对比变化，找出需要更新部分的算法称之为**diff算法**

- 对比策略:

  - 在前面我们实现了```_render```方法,它能够将虚拟DOM转换成真正的DOM

  

  - 但是我们需要改进它,不要让它傻乎乎的重新渲染整个DOM树,而是找出真正变化的部门进行替换。

  

  - 这部门很多类似React框架实现方式都不太一样，有的框架会选择保存上次渲染的虚拟DOM，然后对比虚拟DOM前后的变化，得到一系列更新的数据，然后再将这些更新应用到真正的DOM上。

  

  - 我们会选择直接对比虚拟DOM和真实DOM，这样就不需要额外保存上一次渲染的虚拟DOM，并且能够一边对比一边更新，这也是我们选择的方式。**

  

  - 不管是DOM还是虚拟DOM，他们的结构都是一棵树，完全对比两棵树变化的算法时间复杂度是0(n^3),但是考虑到我们很少会跨层级移动DOM，所以我们只需要对比同一层级的变化。

  ![avatar](https://upload-images.jianshu.io/upload_images/5518628-d60043dbeddfce8b.png?imageMogr2/auto-orient/strip|imageView2/2/w/504/format/webp.png)

  

  <center>只需要对比同一颜色框内的节点</center>

  - 总而言之，我们的**diff算法有两个原则**
    - 对比当前真实的DOM和虚拟DOM，再对比过程中直接更新真实DOM
    - 只对比同一层级的变化

- 5.异步的setState

- <a href="https://www.babeljs.cn/">babel 官网</a>
