---
title: 'CAF：中小型企业 - 成本管理演化  '
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 2/11/2019
description: 阐释：中小型企业 - 成本管理演化
author: BrianBlanchard
ms.openlocfilehash: ca070bfca3efbf9548faa8cf28a1adffefd4a33a
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/08/2019
ms.locfileid: "55900460"
---
# <a name="small-to-medium-enterprise-cost-management-evolution"></a>中小型企业：成本管理演变

本文通过将成本控制添加到治理 MVP 中，对叙述性内容进行了演化。

## <a name="evolution-of-the-narrative"></a>叙述性内容的演化

采用率已超过治理 MVP 中定义的成本容差指标。 这是一件好事，因为这符合从“DR”数据中心迁移后出现的结果。 现在，支出增加证明了云治理团队的时间投入是合理的。

### <a name="evolution-of-the-current-state"></a>当前状态的演化

在此叙述性内容的前一阶段，IT 已经完全停用了 DR 数据中心。 应用程序开发团队和 BI 团队已对生产流量做好准备。

此后，一些变化将会影响治理：

- 迁移团队已开始将 VM 迁移到生产数据中心外。
- 应用程序开发团队正在积极通过 CI/CD 管道将生产应用程序推送到云中。 这些应用程序可以根据用户需求相应缩放。
- IT 中的商业智能团队已在云中交付了许多预测分析工具。 云中聚合的数据量持续增长。
- 所有此类增长都为实现承诺的业务成效提供支持。 不过，成本已开始迅速增加。 预估预算的增长速度快于预期。 CFO 需要使用改进后的成本管理方法。

### <a name="evolution-of-the-future-state"></a>未来状态的演化

成本监视和报告要被添加到云解决方案中。 IT 仍负责成本结算。 也就是说，云服务付款继续来自 IT 采购。 不过，报告应将直接运营费用与产生云成本的职能部门关联起来。 此模型称为云核算的“重现”模型。

当前状态和未来状态的变化带来需要新策略声明的新风险。

## <a name="evolution-of-tangible-risks"></a>有形风险的演化

**预算控制**：自助服务功能本身所具有的风险是，导致新平台的成本意外过多。 为了确保继续与计划预算保持一致，必须建立监视成本和缓解持续成本风险的治理流程。

此业务风险可以扩大成为几种技术风险：

- 实际成本可能超出计划。
- 业务条件发生变化。 如果发生，便会出现业务职能部门需要使用比预期更多的云服务的情况，进而导致支出异常。 存在将这种额外支出视为超额，而不对计划进行必要调整的风险。
- 系统可能预配过度，导致支出过高。

## <a name="evolution-of-the-policy-statements"></a>策略声明的演化

以下策略变化有助于缓解新风险和指导如何实现。

1. 治理团队应每周对照计划监视一次所有云成本。 应每月与 IT 领导层和财务部门共享一次云成本与计划偏差报告。 IT 领导层和财务部门应每月检查一次所有云成本和计划更新。
2. 为了实现问责，必须将所有成本都分配到业务职能部门。
3. 应持续监视云资产，以发现优化机会。
4. 云治理工具必须将“资产规模”选项限制为批准的配置列表。 此工具必须确保所有资产都是可发现的，并受成本监视解决方案跟踪。
5. 在部署计划期间，应记录与托管生产工作负载相关的任何必需云资源。 此项记录有助于优化预算，并做好采用额外自动化的准备，以防使用费用更高的选项。 在此过程中，应考虑云提供商提供的不同折扣工具，如预留实例或许可证成本降低。
6. 为了更好地控制云成本，所有应用程序所有者都必须参加关于优化工作负载的实践培训。

## <a name="evolution-of-the-best-practices"></a>最佳做法的演化

本文的这一部分将把治理 MVP 设计演化为，包括新 Azure 策略和实现 Azure 成本管理。 这两项设计变更将共同实现新的企业策略声明。

1. 实现 Azure 成本管理
    1. 建立适当的访问权限范围，与订阅模式和资源一致性规范保持一致。 假设与先前几篇文章中定义的治理 MVP 保持一致，这就要求对执行简要报告的云治理团队建立“注册帐户范围”访问权限。 其他非治理团队可能需要“资源组范围”访问权限。
    2. 在 Azure 成本管理中设定预算。
    3. 查看并采纳初始建议。 建立支持报告的重复流程。
    4. 配置和执行 Azure 成本管理报告，包括初始报告和重复报告。
2. 更新 Azure Policy
    1. 审核标记、管理组、订阅和资源组值，以确定是否有任何偏差。
    2. 设定 SKU 大小选项，将部署限制为采用部署计划文档中列出的 SKU。

## <a name="conclusion"></a>结束语

将这些流程和变更添加到治理 MVP 中，有助于缓解许多与成本治理相关的风险。 它们共同实现了控制成本所需的可见性、问责和优化。

## <a name="next-steps"></a>后续步骤

随着云采用不断演化和实现更多业务增值，风险和云治理需求也在发生演化。 对于治理之旅中的这家虚构公司，下一步是使用此治理投资来管理多个云。

> [!div class="nextstepaction"]
> [多云演化](./multi-cloud-evolution.md)