# Pattern: Client-side service discovery

## Context
服务（之间）通常需要互相调用。在单体应用中，服务通过语言级别的方法（Method）或过程（Procedure）访问进行互相调用。在一个传统的分布式系统部署中，服务运行在固定的，众所周知的位置（hosts和ports），因此可以使用HTTP/REST或RPC机制方便的进行互相访问。但是，一个现代的基于微服务的应用通常运行在虚拟化的或容器化的环境中，服务实例的数目和他们的位置是动态变化的。
