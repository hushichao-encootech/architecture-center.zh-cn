---
title: "使用 Kubernetes 在 Azure 中设计、构建和操作微服务"
description: "在 Azure 中设计、构建和操作微服务"
author: MikeWasson
ms.date: 12/08/2017
ms.openlocfilehash: 857e91a8eeefec18b459f2e66fde9a4f8bbe7b21
ms.sourcegitcommit: 744ad1381e01bbda6a1a7eff4b25e1a337385553
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2018
---
# <a name="designing-building-and-operating-microservices-on-azure"></a>在 Azure 中设计、构建和操作微服务

![](./images/drone.svg)

微服务已成为一种流行的体系结构类型，可用于构建可复原、高度可缩放、可独立部署且能快速演变的云应用程序。 但是，微服务不仅仅是一种潮流，我们还需要利用不同的方法来设计和构建应用程序。 

在本系列文章中，我们将探讨如何在 Azure 中构建和运行微服务体系结构。 主题包括：

- 使用域驱动的设计 (DDD) 来设计微服务体系结构。 
- 为计算、存储、消息传递和其他设计元素选择适当的 Azure 技术。
- 了解微服务设计模式。
- 复原能力、可伸缩性和性能设计。
- 构建 CI/CD 管道。


整套文章侧重于一个端到端方案：无人机交付服务，可让客户安排通过无人机收寄包裹。 可以在 GitHub 上找到参考实现的代码

[![GitHub](../_images/github.png) 参考实现][drone-ri]

首先，让我们学习一些基础知识。 什么是微服务，采用微服务体系结构可获得哪些优势？

## <a name="why-build-microservices"></a>为何要构建微服务？

在微服务体系结构中，应用程序由小型的独立服务构成。 下面是微服务的一些典型特征：

- 每个微服务实现单个业务功能。
- 微服务很小，一个小规模的开发人员团队就能编写和维护它。
- 微服务在单独的进程中运行，通过妥善定义的 API 或消息传递模式进行通信。 
- 微服务不共享数据存储或数据架构。 每个微服务负责管理自身的数据。 
- 微服务有独立的代码库，且不共享源代码。 不过，它们共用实用工具库。
- 可以独立部署和更新每个微服务。 

如果一切正常，则微服务可以提供诸多有利的优势：

- **敏捷性。** 由于微服务是独立部署的，因此我们可以更轻松地管理 bug 修复和功能发布。 无需重新部署整个应用程序即可更新服务，出现问题时可回滚更新。 在许多传统应用程序中，如果在应用程序的某个部分发现 bug，则整个发布过程可能会停滞；因此，只能在等到修复 bug 之后，才能集成、测试和发布新功能。  

- **小代码，小团队。** 微服务应该足够小，单个功能团队就能构建、测试和部署。 小型代码库更容易理解。 在大型整体应用程序中，经过一段时间后，代码依赖项往往会变得混杂，因此，添加新功能需要在多个位置改动代码。 如果不共享代码或数据存储，则微服务体系结构可将依赖项减到最少，使新功能的添加变得更容易。 小型团队的规模也能大幅提升敏捷性。 “两块披萨的原则”是指团队的规模应该足够小，两块披萨就能让他们吃饱。 显然，这并不是一个确切的指标，因为食量还取决于团队成员的胃口。 但要点在于，大型团队的工作效率往往更低，因为沟通效率较低，管理开销上升，敏捷性下降。  

- **混合技术**。 团队可以选择最适合其服务的技术，并适当地使用混合技术堆栈。 

- **复原能力**。 单个微服务在不可用的情况下不会中断整个应用程序，但前提是所有上游微服务能够正确处理故障（例如，通过实施断路机制）。

- **可伸缩性**。 微服务体系结构允许每个微服务独立进行缩放。 这样，我们便可以横向扩展需要更多资源的子系统，而无需横向扩展整个应用程序。 如果将服务部署在容器中，则还可以将更高密度的微服务打包到一台主机中，从而提高资源的利用效率。

- **数据隔离**。 执行架构更新要容易得多，因为只会影响单个微服务。 在整体应用程序中，架构更新可能极具挑战性，因为应用程序的不同部分可能都要取用相同的数据，对架构进行任何更改都会带来风险。
 
## <a name="no-free-lunch"></a>没有免费的午餐

这些好处不是白给的。 本系列文章旨在解决构建弹性、可缩放且易于管理的微服务时存在的一些难题。

- **服务边界**。 构建微服务时，需要认真考虑如何划分服务之间的边界。 一旦服务构建完成并部署到生产环境中，就很难跨越这些边界进行重构。 设计微服务体系结构时，选择适当的服务边界是最大的难题之一。 每个服务应该有多大？ 何时应该将功能划分到多个服务，何时应该将功能保留在同一个服务中？ 本指南将会介绍使用域驱动的设计找出服务边界的方法。 首先将会介绍如何使用[域分析](./domain-analysis.md)找出界定上下文，然后根据功能性和非功能性要求应用一组[战术性 DDD 模式](./microservice-boundaries.md)。 

- **数据一致性和完整性**。 微服务的基本原则是每个服务管理其自身的数据。 这可以保持服务的分离性，但也可能会在数据完整性或冗余方面带来挑战。 [数据注意事项](./data-considerations.md)中将会探讨其中的某些问题。

- **网络拥塞和延迟**。 使用大量小型的精细服务可能会增加服务间的通信量，并增大端到端的延迟。 [服务间通信](./interservice-communication.md)一章介绍了有关服务之间的消息传递的注意事项。 同步和异步通信在微服务体系结构中很有作用。 良好的 [API 设计](./api-design.md)非常重要，既要让服务保持松散耦合，同时也要让它们能够独立部署和更新。
 
- **复杂性**。 微服务应用程序包含更多的运动部件。 每个服务可能比较简单，但服务必须能够作为一个整体协同工作。 单个用户操作可能涉及到多个服务。 [引入和工作流](./ingestion-workflow.md)一章介绍了有关以较高吞吐量引入请求、协调工作流以及处理故障的一些问题。 

- **客户端与应用程序之间的通信。**  将某个应用程序分解成许多小型服务时，客户端应如何与这些服务通信？ 客户端是要直接调用每个服务，还是通过 [API 网关](./gateway.md)路由请求？

- **监视**。 监视分布式应用程序可能比监视整体应用程序要难得多，因为必须关联来自多个服务的遥测数据。 [日志记录和监视](./logging-monitoring.md)一章介绍了如何解决这些难题。

- **持续集成和交付 (CI/CD)**。 微服务的主要目标之一是敏捷性。 若要实现此目标，必须采用自动且可靠的 [CI/CD](./ci-cd.md)，以便快速可靠地将各个服务部署到测试和生产环境。

## <a name="the-drone-delivery-application"></a>无人机交付应用程序

为了探讨这些问题并演示微服务体系结构的一些最佳做法，我们创建了一个名为“无人机交付应用程序”的参考实现。 可以在 [GitHub][drone-ri] 上找到该参考实现。

Fabrikam, Inc. 正在推出无人机交付服务。 该公司经营无人机群。 各商家注册该服务，用户可以请求无人机收取要交付的商品。 当客户安排取件时，后端系统会分配一架无人机，并将估计的交付时间告知用户。 在交付过程中，客户可以通过持续更新的 ETA 跟踪无人机的位置。

此方案涉及到一个相当复杂的域。 部分业务难题包括安排无人机、跟踪包裹、管理用户帐户，以及存储和分析历史数据。 此外，Fabrikam 希望快速投放市场和扩张，同时添加新功能。 该应用程序需要以云的规模运行，并附带较高的服务级别目标 (SLO)。 此外，Fabrikam 预期系统的不同部件在数据存储和查询方面具有截然不同的要求。 所有这些考虑因素促使 Fabrikam 为无人机交付应用程序选择了微服务体系结构。

> [!NOTE]
> 如需在微服务体系结构与其他体系结构样式之间做出选择的帮助，请参阅 [Azure 应用程序体系结构指南](../guide/index.md)。

我们的参考实现将 Kubernetes 与 [Azure 容器服务 (ACS)](/azure/container-service/kubernetes/) 配合使用。 但是，许多高层面的体系结构决策和难题也适用于任何容器业务流程协调程序，包括 [Azure Service Fabric](/azure/service-fabric/)。 

> [!div class="nextstepaction"]
> [域分析](./domain-analysis.md)


<!-- links -->

[drone-ri]: https://github.com/mspnp/microservices-reference-implementation