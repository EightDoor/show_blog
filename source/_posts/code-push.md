---
title: code-push
date: 2020-01-14 10:30:55
tags: 前端
categories: react-native
---

1. code-push 常用命令
   安装: npm install -g code-push-cli
   注册账号: code-push register
   登陆: code-push login
   注销: code-push logout
   添加项目: code-push app add [app 名称]
   删除项目: code-push app remove [app 名称]
   列出账号下的所有项目: code-push app list
   显示登陆的 token: code-push access-key ls
   删除某个 access-key: code-push access-key rm <accessKey>
   添加协作人员：code-push collaborator add <appName> next@126.com
   部署一个环境: code-push deployment add <appName> <deploymentName>
   删除部署: code-push deployment rm <appName>
   列出应用的部署: code-push deployment ls <appName>
   查询部署环境的 key: code-push de ployment ls <appName> -k
   查看部署的历史版本信息: code-push deployment history <appName> <deploymentNmae>
   重命名一个部署: code-push deployment rename <appName> <currentDeploymentName> <newDeploymentName>
   打包 bundle 包 react-native bundle --entry-file index.js --bundle-output ./bundle/android/index.android.bundle --platform android --assets-dest ./bundle/android --dev false
   发布更新版本 (直接推送到生产环境) code-push release-react newDemo android -t 1.0.0 --dev false --d Production --des "这是第一个" -m true
   2.cordova 常用命令
   // 发布 Staging 版本
   \$ code-push release-cordova <appName> ios

// 发布 Production 版本
\$ code-push release-cordova <appName> ios -d Production

// 查看已发布的版本
\$ code-push deployment ls <appName> -k

//给 app 在热更新服务器上创建应用
code-push app add <appName> <os> <platform>

//删除应用
code-push app rm <appName>

//查看热更新服务器上有哪些应用
code-push app list

//发布应用
code-push release-cordova <appName> <platform> [options]
Options 参数:
--deploymentName, -d ..指定部署的类型.默认"Staging",可以选择"Production"或其他 自定义类型
--description, --des ..添加描述
--mandatory, -m .......指定此版本是否为强制更新版本
例 1:发布更新
code-push release-cordova ionic2_tabs_android android --des ""
例 2:部署"Production"状态的更新,即生产环境的热更新部署使用这句命令
code-push release-cordova ionic2_tabs_android android -d "Production" --des ""
注意:一般生产环境的 app 是压缩过的,所以在发布正式环境热更新之前,先执行"ionic build --prod"压缩代码
例 3:部署 ios 应用的更新
code-push release-cordova ionic2_tabs_ios ios --des ""
例 4:添加-m 参数强制更新,code-push 插件从服务端下载完代码,会立即自动重启 app
code-push release-cordova ionic2_tabs_android android -m --des ""

//查看部署状态
code-push deployment list <appName>
例 1:
code-push deployment list ionic2_tabs_android
例 2:查看部署状态及 key 值,忘记 key 就这样找
code-push deployment list ionic2_tabs_android -k

//清空部署记录
code-push deployment clear <appName> <deploymentName>
如:清空 Staging 状态的部署记录
code-push deployment clear ionic2_tabs_android Staging

//添加部署状态,默认只有"Staging"和"Production"两中状态
code-push deployment add <appName> [deploymentName]

//删除自定义的部署状态
code-push deployment rm <appName> <deploymentName>
