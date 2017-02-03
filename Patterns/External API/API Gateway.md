
# Pattern: API Gateway

## Context

想象一下你正在使用微服务架构模式（Microservice architecture pattern）构建一个在线商城，你当前正在实现一个产品明细页面。你需要开发多个版本的产品明细用户接口（User Interface）：
* 基于HTML5/JavaScript的桌面和移动端浏览器UI - 服务端Web应用生成HTML
* Native Android和iPhone客户端 - 这些客户端通过REST APIs跟服务端进行交互
