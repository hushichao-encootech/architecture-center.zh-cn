---
title: "守护程序"
description: "通过使用专用的主机实例保护应用程序和服务，该实例用于充当客户端和应用程序或服务之间的中转站、验证和整理请求，并在它们之间传递请求和数据。"
keywords: "设计模式"
author: dragon119
ms.date: 06/23/2017
pnp.series.title: Cloud Design Patterns
pnp.pattern.categories: security
ms.openlocfilehash: 39f8548bbccb5e19d433f65b2e7e09147d676996
ms.sourcegitcommit: b0482d49aab0526be386837702e7724c61232c60
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2017
---
# <a name="gatekeeper-pattern"></a>守护程序模式

[!INCLUDE [header](../_includes/header.md)]

通过使用专用的主机实例保护应用程序和服务，该实例用于充当客户端和应用程序或服务之间的中转站、验证和整理请求，并在它们之间传递请求和数据。 这可以额外提供一层安全，并限制系统的受攻击面。

## <a name="context-and-problem"></a>上下文和问题

应用程序通过接受和处理请求向客户端公开其功能。 在云托管方案中，应用程序公开客户端连接，并通常包括代码以处理来自客户端的请求。 此代码执行身份验证和验证、部分或全部请求处理，并可能代表客户端访问存储和其他服务。

如果恶意用户能够危害系统并访问应用程序的托管环境，那么它所使用的安全机制（例如凭据和存储密钥）以及它可访问的服务和数据访问都会暴露出来。 因此，恶意用户可以无限制地访问敏感信息和其他服务。

## <a name="solution"></a>解决方案

为了最大限度地减少客户端访问敏感信息和服务的风险，请将公共终结点的主机或任务与处理请求和访问存储的代码分离。 可通过使用某个门面或专用任务完成此操作，该门面或专用任务与客户端交互，然后将请求&mdash;可能通过一个分离的接口&mdash;提交到将要处理该请求的主机或任务。 图提供了此模式的综合概述。

![此模式的综合概述](./_images/gatekeeper-diagram.png)


守护程序模式可用于简单地保护存储，或者可用作更全面的机制来保护应用程序的所有功能。 重要因素是：

- **受控的验证。** 守护程序验证所有请求，并拒绝不满足验证的要求。
- **有限的风险和公开。** 守护程序不能访问受信任主机访问存储和服务所用的凭据或密钥。 如果守护程序受到攻击，攻击者无权访问这些凭据或密钥。
- **适当的安全。** 守护程序在有限特权模式下运行，而其他应用程序在访问存储和服务所需的全信任模式下运行。 如果守护程序受到攻击，那么它不能直接访问应用程序服务或数据。

此模式就像典型网络拓扑中防火墙。 它允许守护程序检查请求，并决定是否将请求传递到执行所需任务的受信任主机（有时称为 keymaster）。 此决策通常需要守护程序验证和整理请求内容，然后再将它传递到受信任主机。

## <a name="issues-and-considerations"></a>问题和注意事项

在决定如何实现此模式时，请考虑以下几点：

- 请确保接受守护程序传递请求的受信任主机只公开到内部或受保护的终结点，并只连接守护程序。 受信任的主机不应公开任何外部终结点或接口。
- 守护程序必须在有限特权模式下运行。 通常这意味着在分离托管的服务或虚拟机中运行守护程序和信任的主机。
- 守护程序不应执行与应用程序或服务有关的任何处理或访问任何数据。 其作用仅是用于验证并整理请求。 受信任的主机可能需要执行请求的其他验证，但应通过守护程序执行核心验证。
- 在守护程序和受信任的主机或任务（在可能的情况下）之间使用安全通信通道（HTTPS、SSL或TLS）。 但是，某些宿主环境在内部终结点上不支持 HTTPS。
- 为实现实现守护程序，需要将此附加层添加到应用程序，这可能会对性能造成影响（因为它需要额外的处理和网络通信）。
- 守护程序实例可能是单点故障。 若要尽量减少失败的影响，请考虑部署其他实例并使用自动缩放机制以确保维护可用性的能力。

## <a name="when-to-use-this-pattern"></a>何时使用此模式

此模式适合用于：

- 处理敏感信息、公开服务（此类服务必须具有很高级别保护以防止恶意攻击）或执行不应中断的任务关键型操作的应用程序。
- 分布式应用程序，它需要与主要任务分开执行请求验证，或集中该验证以简化维护和管理。

## <a name="example"></a>示例

在云托管方案中，可以通过将守护程序角色或虚拟机与应用程序中的受信任角色和服务分离来实现此模式。 将内部终结点、队列或存储用作中间通信机制执行此操。 本图演示了如何使用内部终结点。

![使用云服务 web 和辅助角色的模式示例](./_images/gatekeeper-endpoint.png)


## <a name="related-patterns"></a>相关模式

当执行守护程序模式时，[Valet 密钥模式](valet-key.md)可能也相关。 在守护程序和受信任的角色之间通信时，可通过限制访问资源权限的密钥或令牌增强安全。 描述如何使用令牌或密钥让客户端以有限方式访问特定资源或服务。