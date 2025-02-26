---
layout: mypost
title: "Year 2 Project: 前端开发"
categories: [Year 2 Project]
lang: en-us

---

Link to [English Version](https://juntong20xx.github.io/posts/2025/02/22/Year-2-Project-Final-Frontend-Development.html).

![](https://miro.medium.com/v2/0*MAYv38yGNqZa4K1R.)



进行前端调试。

项目采用了前后端分离的设计，前端采用 Vue 框架，为了开发方便，使用 `node.js` 作为开发环境。

## 项目结构设计

```
.
├─public
├─src
│   ├─assets
│   └─components
├─index.html
├─jsconfig.json
├─package.json
└─vite.config.js
```

将每一个功能作为独立页面，每个页面是一个 Vue Component 文件。

从功能上，分成两部分：控制界面和登录界面。

## 控制页面

控制界面的目的是实时显示连接的模块名称，传感器参数以及允许用户执行操作。

### 前端设计

将数据显示在一个表格中。由于设备是可以动态增减的，依托 Vue 的动态响应特性，表格也会实时更新。

对不同的操作提示使用不同的颜色，如"Update"操作使用绿色，而"Logout"使用红色；执行成功的提示关闭按键为绿色，执行失败的关闭按键为红色。

### API 设计

在控制界面可以简单分为两类：获取模块列表和提交动作。使用 get 方法获取，通过 put 方法提交更新。

为了方便与后端通信，引入了 `axios` 模块.

```javascript
await axios.put(`${this.backendUrl}/items`, {
	key: item.key,
  value: item.value
})
```

### 测试问题和解决方案

需要实时刷新：

解决方案：登录后添加定时器函数函数获取后端列表并刷新。

用户输入中表格刷新

解决方案：判断用户是否聚焦或用户更改值，若是则避免刷新，注意模块离线时仍会移除这一行。

由于更改值但未提交导致显示与真实参数不一致

解决方案：当值不一致时，添加提示文字。

### 界面展示

控制界面

![Screen](Control Page.png)

当更改但未提交时的界面

![image-20250226004301640](Control Page with Update.png)

## 登录界面

为了确保安全性，使用 TOTP 取代密码。并且用户名是可以自定义的，因此用户名可以一定程度上替代密码。从这个角度系统进行了强制双因素认证。

为了增加前后端分离的灵活性，要求用户指定后端，浏览器将保存后端位置，并向后端发送验证请求和后续控制操作。

### 前端设计

常规的登录表单，需要用户输入 用户名 TOTP 和 后端网址。

### API 设计

向用户输入的后端地址，发送包含用户名和 TOTP 的表单。后端返回验证结果和 Token。

### 测试问题和解决方案

错误的后端导致界面卡死

解决方案：在点击登录后添加定时器，若遇到端口关闭或长时间未响应，则停止尝试并提示用户检查后端地址。

### 界面展示

![Login Page]( Login Page.png)

## 发布

使用 `node.js` 的 build 指令创建项目，使用 nginx 或 cloudflare 部署生成的 dist 目录。

![Pages](Page GIF.gif)
