---
title: CAS 单点登录原理介绍
date: 2017-05-20 21:37:32
tags:
---

本文主要介绍cas实现单点登录的原理，对登录过程的细节进行梳理，期望达到的效果：

- 帮助正在做技术选型的的人做一个简单的了解（文章后面会介绍cas使用过程中遇到的一些问题）
- 帮助正在使用的人理清登录过程的细节
- 深入理解cas认证过程，并借鉴到其他场景

**单点登录**

单点登录（Single Sign On），简称为 SSO，是目前比较流行的企业业务整合的解决方案之一。SSO的定义是在多个应用系统中，用户只需要登录一次就可以访问所有相互信任的应用系统。 [ 摘选于[百度百科][1] ]



**CAS 简介**

CAS 是 Yale 大学发起的一个开源项目，旨在为 Web 应用系统提供一种可靠的单点登录方法，CAS 在 2004 年 12 月正式成为 JA-SIG 的一个项目，地址：https://www.apereo.org/projects/cas


**单系统登录与会话保持**

因为http是无状态的协议，会话保持一般是基于cookie。
最简单的方式，系统给客户端分配一个id，通过http response发送给客户端，客户端下一次访问系统时会携带这个id，系统通过id来识别用户的身份。
（对照java体系，就是所谓的session和jsessionid）

![客户端&服务器端状态保持流程][2]

**CAS 实现单点登录的基本思路**

- 在需要单点登录的多个业务系统之外引入单独的web系统（cas server）
- Cas server统一校验用户身份，维护用户登录状态
- 用户需要进入第三方系统时，需要携带 Cas server 颁发的证书到业务系统
- 业务系统收到证书后向cas server发送请求校验证书是否合法，如果合法，即登录成功




**CAS 登录过程**

背景设定

| 系统名称 | 系统地址 |
| -------- | -------- |
|    用户管理      |  www.a.com        |
|    订单管理      |  www.b.com        |
|    cas server      | www.c.com         |

**用户第一次打开系统**
用户小明9点钟到公司，打开浏览器，输入订单系统地址，准备查看昨天上报来的订单信息。

*cas是基于浏览器cookie来实现用户持有票据的，我们会跟踪小明每一步操作前后的cookie状态*

此时浏览器cookie状态为

| 域名 | cookie key |     |
| ---- | ---------- | --- |
|   -   |     -       |  -   |

嘀嘀嘀





  [1]: http://baike.baidu.com/item/%E5%8D%95%E7%82%B9%E7%99%BB%E5%BD%95?fr=aladdin
  [2]: http://i1.piimg.com/594918/d26485939b567f79.jpg
