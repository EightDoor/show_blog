---
title: taro多端开发
date: 2020-01-29 21:31:56
tags: 前端
categories: taro
---

# taro 多端开发应用

## 在线预览地址

- H5 <a href="https://taro.start6.cn">地址</a>

## egg 后台

- 问题总结:

  - egg 跨域:

    - ```javascript
      步骤一：
      # 下载 egg-cors 包
      npm i egg-cors --save

      步骤二：
      # 在plugin.js中设置开启cors
      exports.cors = {
        enable: true,
        package: 'egg-cors',
      };

      步骤三：
      # 在config.{env}.js中配置，注意配置覆盖的问题
      config.security = {
        csrf: {
          enable: false,
          ignoreJSON: true
        },
        domainWhiteList: '*'
      };

      config.cors = {
        origin:'*',
        allowMethods: 'GET,HEAD,PUT,POST,DELETE,PATCH'
      };
      ```

## 今日头条接口

1. 首页顶部 tabs 接口: `https://api3-normal-c-lf.snssdk.com/article/category/get_subscribed/v4/?iid=101398257036&device_id=66877782274&ac=wifi&mac_address=A4%3A50%3A46%3ACD%3A3D%3AAB&channel=xiaomi&aid=13&app_name=news_article&version_code=757&version_name=7.5.7&device_platform=android&ab_version=1251921%2C662099%2C1407070%2C668774%2C1396152%2C1445075%2C765196%2C821967%2C857803%2C660830%2C1439346%2C1397711%2C1243993%2C1434500%2C1379677%2C662176%2C1378615%2C801968%2C1419048%2C668775%2C1190524%2C1157750%2C1419597%2C1439625%2C1422304%2C1428576%2C668779%2C759656%2C1388002&ab_feature=94563%2C102749&ssmix=a&device_type=MI+8&device_brand=Xiaomi&language=zh&os_api=28&os_version=9&uuid=869832047288317&openudid=c836b6236d8fcd86&manifest_version_code=7571&resolution=1080*2029&dpi=440&update_version_code=75717&_rticket=1580206995760&plugin=18762&pos=5r_-9Onkv6e_eCopeCA7eyoLeC0JeCUfv7G_8fLz-vTp6Pn4v6esraSzrKuvrq6tqaWtr66uqqWxv_H86fTp6Pn4v6eprLOpqqqrrqWrq6uprKqlpbG__PD87d706eS_p794Kil4IDt7Kgt4LQl4JR-_sb_88Pzt0fLz-vTp6Pn4v6esraSzrKuvrq6tqaWtr66uqqWxv_zw_O3R_On06ej5-L-nqayzqaqqq66lq6urqayqpaXg&rom_version=miui_v11_v11.0.4.0.peacnxm&cdid=ee711002-293a-4f81-8060-8ffa05e9a76f&oaid=4aea438a0899600d`
2. 首页顶部->推荐接口: `https://api3-normal-c-lf.snssdk.com/api/news/feed/v88/`

## 开源中国接口

1. 综合->顶部->软件:

   - 列表: `https://h5.oschina.net/apiv3/projectRecommend?size=20&page=1`

   - 详情: `https://h5.oschina.net/project/detail/50356`

## 京东万象 api 接口

- api 地址: <a href="https://wx.jdcloud.com/market/api/11073">新闻</a>
