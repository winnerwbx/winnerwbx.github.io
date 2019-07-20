---
title: ARTS-WEEK-1-Review
date: 2019-07-20 18:46:41
tags: ARTS Review
---
## Review

[What it means to be Cloud Native approach — the CNCF way](https://medium.com/developingnodes/what-it-means-to-be-cloud-native-approach-the-cncf-way-9e8ab99d4923)

本文是实现云原生的科普文，主要介绍了云原生、云原生计算基金会（CNCF，Cloud Native Computing Foundation），实现云原生的路线图，以及现在有哪些云原生的开源产品。

### 什么是云原生和CNCF？

云原生是指充分利用云计算技术进行服务部署和运行的一种实现手段。

云原生的架构能够充分利用云计算的技术以实现按需部署、弹性扩缩容、持续集成和交付，并能较好地节约服务成本。

有了容器技术才有云原生，Google的Kubernetes项目目前已经成为容器编排界的事实标准，2015年，Google把Kubernetes贡献给了CNCF，标志着CNCF的诞生。

CNCF认为云原生具有如下三个特点：

1. 使用容器技术、微服务和声明式API
2. 运行在动态环境上（云基础设施）
3. 服务是可伸缩可扩展的（Scalable）

### 实现云原生的路线图

根据业务需求，有很多手段可以实现云原生，CNCF给出了一些建议的工具和最佳实践，以实现云原生。

具体分以下几点：

1. 容器化
    
    容器技术把服务所需要的代码和所有依赖打包到container中，几乎运行服务所需要的一切都可以打包到容器里。
    
    这里最常用的就是Docker了。
        
    有了Docker的container，就可以实现按需部署和扩缩容了。
    
2. CI/CD（持续集成、持续部署）
    
    CI/CD是指对代码的改动提交到代码仓库（如git等），自动执行构建、打包、生成container、部署的一个完整的自动化流程。支持自动化发布、回滚和测试等。
    
    常用的工具如Jenkins等。
    
3. 容器编排

    容器编排是指管理诸多container的生命周期。使用容器便来来控制和自动化一些任务。
    
    使用最广泛的当然就是Kubernetes了
    
4. 监控和分析

    Kubernetes不提供原生的日志解决方案，但是有许多可用的工具可以实现对服务的监控、日志收集以及链路追踪。
    
    如：Prometheus（用于监控）、fluentd（用于日志）以及Jaeger（用于链路追踪）等。

5. Service Mesh（服务网格）
    
    Service Mesh是将服务注册发现、健康管理、路由等功能从服务本身实现中抽离出来，提供一个统一的能力。
    
    服务无需关心这些细节，只需与本地的client进行通信，由Service Mesh的client进行服务注册发现、健康管理等。
    
    常用工具有：Istio、CoreDNS、Envoy和Linkerd
    
6. 分布式数据库

    云原生也提供了分布式数据库的一些方案，如使用Vitess、Rook等。
    
7. 服务调用

    除了传统的JSON-REST方式，也可以使用gRPC、NATS等。
    
8. 容器注册和运行

    容器注册中心用于容器image的管理、权限控制等，如Docker hub、Nexus registry等
    
    containerd是容器的运行时，管理了单机上容器的生命周期。
    
### 总结

云原生从一开始就基于为运行在云计算基础设施上的服务提供支持。

带来了哪些好处？

服务的高可用、弹性扩缩容、快速响应能力，自治能力和故障恢复能力，也许这就是它越来越流行的原因吧。