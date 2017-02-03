
# Pattern: API Gateway

## Context

想象一下你正在使用微服务架构模式（Microservice architecture pattern）构建一个在线商城，你当前正在实现一个产品明细页面。你需要开发多个版本的产品明细用户接口（User Interface）：
* 基于HTML5/JavaScript的桌面和移动端浏览器UI - 服务端Web应用生成HTML
* Native Android和iPhone客户端 - 这些客户端通过REST APIs跟服务端进行交互

另外，在线商城还必须暴露产品明细REST API给第三方应用使用。

一个产品明细UI要能够展示大量的产品信息。举例来说，Amazon.com上[《POJOs in Action》](https://www.amazon.com/POJOs-Action-Developing-Applications-Lightweight/dp/1932394583)明细页面展示如下信息：
* 书籍的基础信息，比如，标题，作者，价格，等。
* 你的书籍购买历史
* 产品可获得性信息（Availability）
* 购买选项（Buying option）
* 经常跟这本书一起买的商品列表
* 购买这本书的顾客也同时购买商品列表
* 顾客评论（Customer Reviews）
* 销售排行版
* ......

因为在线商城使用微服务架构模式，因此产品明细数据会涵盖多个服务。
举例来说，
* 产品信息服务 - 产品基础信息，例如标题，作者
* 价格服务 - 产品价格
* 订单服务 - 产品购买历史
* 库存服务 - 产品可获得性信息
* 评论服务 - 顾客评论
* ......

因此，展示产品明细的代码必须从所有这些服务中获取信息。

## Problem
如何让这些基于微服务应用的客户端访问各种个性化服务（Individual Services）？

## Forces
