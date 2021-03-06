---
title: 网关路由模式
titleSuffix: Cloud Design Patterns
description: 使用单个终结点将请求路由到多个服务。
keywords: 设计模式
author: dragon119
ms.date: 06/23/2017
ms.topic: design-pattern
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom: seodec18
ms.openlocfilehash: e5c93c98a562e790d547d08fdf312c973cfceed8
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2019
ms.locfileid: "54487215"
---
# <a name="gateway-routing-pattern"></a>网关路由模式

使用单个终结点将请求路由到多个服务。 如果希望在单个终结点上公开多个服务，并根据请求路由到适当的服务，则此模式非常有用。

## <a name="context-and-problem"></a>上下文和问题

客户端需要使用多个服务时，为每个服务设置单独的终结点并让客户端管理每个终结点是具有挑战性的。 例如，一个电子商务应用程序可以提供搜索、评价、购物车、结账和订单历史记录等服务。 每个服务都有一个客户端必须与之交互的不同 API，客户端必须了解每个终结点，以便连接到服务。 如果一个 API 发生变化，那么客户端也必须更新。 如果将一个服务重构为两个或多个单独的服务，则必须在服务和客户端中更改代码。

## <a name="solution"></a>解决方案

在一组应用程序、服务或部署前放置网关。 使用应用层 7 路由将请求路由到相应实例。

使用此模式，客户端应用程序只需了解单个终结点并与之通信。 如果服务进行合并或分解，客户端不一定需要更新。 它可以继续向网关发出请求，只有路由会更改。

使用网关，还可以从客户端提取后端服务，保持客户端调用的简单性，同时在网关后的后端服务中启用更改。 客户端调用可以被路由到任何需要处理预期的客户端行为的服务，无需更改客户端即可在网关后面添加、拆分和重组服务。

![网关路由模式图](./_images/gateway-routing.png)

这种模式允许管理向用户推出更新的方式，可以帮助进行部署。 部署了新版本的服务后，它可以与现有版本并行部署。 通过路由，可以控制向客户端提供哪种版本的服务，能够灵活地使用各种发布策略，无论是递增、并行还是完整的推出更新都可以。 通过在网关上进行配置更改可以快速还原部署新服务后发现的任何问题，不会影响客户端。

## <a name="issues-and-considerations"></a>问题和注意事项

- 网关服务可能会造成单一故障点。 请确保它设计合理，符合用户的可用性需求。 在实施时，请考虑复原和容错能力。
- 网关服务可能会造成瓶颈。 请确保网关有足够的性能来处理负载，并且可以根据增长预期轻松扩展。
- 对网关执行负载测试，确保不会对服务造成级联故障。
- 网关路由是第 7 级。 它可以基于 IP、端口、标头或 URL。

## <a name="when-to-use-this-pattern"></a>何时使用此模式

在以下情况下使用此模式：

- 客户端需要使用可在网关后访问的多个服务。
- 你希望通过使用单个终结点来简化客户端应用程序。
- 需要将请求从外部可寻址的终结点路由到内部虚拟终结点，例如对集群虚拟 IP 地址公开 VM 上的端口。

当存在某个简单应用程序仅使用一两个服务时，此模式可能不适用。

## <a name="example"></a>示例

使用 Nginx 作为路由器，以下为服务器的一个简单示例配置文件，将驻留在不同虚拟目录上的应用程序的请求路由到后端不同的计算机。

```console
server {
    listen 80;
    server_name domain.com;

    location /app1 {
        proxy_pass http://10.0.3.10:80;
    }

    location /app2 {
        proxy_pass http://10.0.3.20:80;
    }

    location /app3 {
        proxy_pass http://10.0.3.30:80;
    }
}
```

## <a name="related-guidance"></a>相关指南

- [用于前端的后端模式](./backends-for-frontends.md)
- [网关聚合模式](./gateway-aggregation.md)
- [网关卸载模式](./gateway-offloading.md)
