
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
* 微服务提供的APIs粒度跟客户端需要的通常是不一样的。微服务通常提供细粒度（fine-grained）的APIs，这意味着客户端需要跟多个服务进行交互。举例来说，如上所述，客户端所需要的产品明细（数据）需要从很多的服务中获取。
* 不同的客户端需要不同的数据。举例来说，桌面浏览器版本的产品明细页面，桌面（版本）通常比移动版本更加复杂。
* 不同客户端的网络性能要求也不同。举例来说，移动网络相较于非移动网络通常更慢，延迟更高。任何WAN比LAN要慢。这意味着，Native客户端使用的网络跟服务端Web应用使用的LAN有着非常不一样的网络特征。服务端Web应用可以发起多个针对后端服务的请求而不影响用户体验，然而移动客户端只能发起少量（请求）。
* 服务实例（Service Instance）的数量和位置（host + port）动态变化
* 服务划分（Partitioning into Services）能够随时变化且应该对客户端透明。

## Solution
实现一个API Gateway作为所有客户端的唯一入口点（single entry point）。API Gateway以两种方式处理请求。一些请求简单的代理（proxy）/路由（route）到对应的服务。其他请求则被展开成多个服务（调用）。
![Use an API gateway](http://microservices.io/i/apigateway.jpg)
而不是提供一个一体适用（one-size-fits-all）的API，相反，API gateway为每个客户端暴露不同的API。举例来说，[Netflix API](http://techblog.netflix.com/2012/07/embracing-differences-inside-netflix.html) Gateway通过特定客户端适配代码（client-specific adapter code）为每个客户端提供最契合它们需求的API。
API Gateway还必须实现安全（Security），例如，验证客户端是否被授权执行请求。

## Example
[Netflix API Gateway](http://techblog.netflix.com/2013/01/optimizing-netflix-api.html)

## Resulting context
使用API Gateway有如下有点：
* 隔离客户端与应用的微服务划分
* 隔离客户端与服务实例位置测定问题
* 为每个客户端提供最适宜的API
* 减少请求往返次数。举例来说，API Gateway允许客户端通过一次（请求）往返获取多个服务的数据。更少的请求意味着更低的负载和提升用户体验。对移动应用来说API Gateway是必不可少的。
* 简化客户端，通过将访问多个服务的逻辑从客户端迁移到API Gateway。
API Gateway模式有一些缺点：
* 增加复杂性 - API Gateway也是另一个活动部件（moving part），需要被开发、部署和管理
* 增加响应时间，由于通过API Gateway而产生的额外网络跃点（Network Hop） - 但是，对于大部分应用来说这些额外的（请求）往返不重要。
问题：
* 如何实现API Gateway？如果需要规模处理高负载，时间驱动/响应的方法（event-driven/reactive approach）是最佳选择。在JVM上，基于NIO的库，例如Netty,Spring Reactor，等。有道理。NodeJS也是一个选择。

## Related patterns
* API Gateway使用[客户端服务发现模式](/Patterns/Service discovery/Client-side discovery.md)（Client-side Discovery pattern）或者[服务端服务发现模式](/Patterns/Service discovery/Server-side discovery.md)（Server-side Discovery pattern）路由请求到可用的服务实例。
* API Gateway需要认证用户并传输包含用户信息的Access Token到服务。
* API Gateway应该使用断路器（Circuit Breaker）调用服务。

## Known uses
* [Netflix API Gateway](http://techblog.netflix.com/2012/07/embracing-differences-inside-netflix.html)

> 原文地址：[Pattern: API Gateway](http://microservices.io/patterns/apigateway.html)
