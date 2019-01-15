---
title: "Weixin"
date: 2018-12-15T07:54:36+08:00
draft: true
---

### 小程序登录

小程序可以通过微信官方提供的登录能力方便地获取微信提供的用户身份标识，快速建立小程序内的用户体系。  
https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/login.html

说明：  
调用 wx.login() 获取 临时登录凭证code ，并回传到开发者服务器。  
调用 code2Session 接口，换取 用户唯一标识 OpenID 和 会话密钥 session_key。  
之后开发者服务器可以根据用户标识来生成自定义登录态，用于后续业务逻辑中前后端交互时识别用户身份。

#### wx.login(Object object)

https://developers.weixin.qq.com/miniprogram/dev/api/wx.login.html

wx.login(Object object)

调用接口获取登录凭证（code）。通过凭证进而换取用户登录态信息，包括用户的唯一标识（openid）及本次登录的会话密钥（session_key）等。用户数据的加解密通讯需要依赖会话密钥完成。更多使用方法详见 小程序登录。


#### code2Session

https://developers.weixin.qq.com/miniprogram/dev/api/code2Session.html

code2Session

> 本接口应在后端服务器调用

登录凭证校验。通过 wx.login() 接口获得临时登录凭证 code 后传到开发者服务器调用此接口完成登录流程。更多使用方法详见 小程序登录。

### 小程序与小游戏获取用户信息接口调整  
https://mp.weixin.qq.com/wxopen/announce?action=getannouncement&announce_id=1524127799&version=2&lang=zh_CN

为优化用户体验，使用 wx.getUserInfo 接口直接弹出授权框的开发方式将逐步不再支持。从2018年4月30日开始，小程序与小游戏的体验版、开发版调用 wx.getUserInfo 接口，将无法弹出授权询问框，默认调用失败。正式版暂不受影响。开发者可使用以下方式获取或展示用户信息：

#### 一、小程序

1、使用 button 组件，并将 open-type 指定为 getUserInfo 类型，获取用户基本信息。  
详情参考文档:  
https://developers.weixin.qq.com/miniprogram/dev/component/button.html

2、使用 open-data 展示用户基本信息。  
详情参考文档:  
https://developers.weixin.qq.com/miniprogram/dev/component/open-data.html  
https://developers.weixin.qq.com/miniprogram/dev/component/button.html