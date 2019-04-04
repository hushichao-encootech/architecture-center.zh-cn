---
title: Azure 中的 R 开发指南 - R 编程 | Microsoft Docs
description: 本文概述数据科学家在 Azure 中利用现有 R 编程语言技能的各种方式。 Azure 提供了很多 R 开发人员可以使用扩展其数据科学工作负载分配到云的服务。
services: machine-learning
author: AnalyticJeremy
ms.service: machine-learning
ms.workload: data-services
ms.devlang: R
ms.topic: article
ms.date: 03/19/2019
ms.author: jepeach
ms.openlocfilehash: a14c8ce76f78baa7274f22b939eb28cb025ef87e
ms.sourcegitcommit: 1a3cc91530d56731029ea091db1f15d41ac056af
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2019
ms.locfileid: "58887958"
---
# <a name="r-developers-guide-to-azure"></a><span data-ttu-id="44f89-104">Azure 中的 R 开发指南</span><span class="sxs-lookup"><span data-stu-id="44f89-104">R developer's guide to Azure</span></span>

<img src="../images/logo_r.svg" alt="R logo" align="right" width="200" />

<span data-ttu-id="44f89-105">处理日益增长的数据量的许多数据科学家正在寻求利用云计算的强大能力进行分析。</span><span class="sxs-lookup"><span data-stu-id="44f89-105">Many data scientists dealing with ever-increasing volumes of data are looking for ways to harness the power of cloud computing for their analyses.</span></span>  <span data-ttu-id="44f89-106">本文概述数据科学家在 Azure 中利用现有 [R 编程语言](https://www.r-project.org)技能的各种方式。</span><span class="sxs-lookup"><span data-stu-id="44f89-106">This article provides an overview of the various ways that data scientists can leverage their existing skills with the [R programming language](https://www.r-project.org) in Azure.</span></span>

<span data-ttu-id="44f89-107">Microsoft 完全接受将 R 编程语言作为数据科学家的第一类工具。</span><span class="sxs-lookup"><span data-stu-id="44f89-107">Microsoft has fully embraced the R programming language as a first-class tool for data scientists.</span></span>  <span data-ttu-id="44f89-108">本公司为 R 开发人员提供许多不同的选项让他们在 Azure 中运行其代码，并使数据科学家在处理大型项目时能够将其数据科学工作负荷扩展到云中。</span><span class="sxs-lookup"><span data-stu-id="44f89-108">By providing many different options for R developers to run their code in Azure, the company is enabling data scientists to extend their data science workloads into the cloud when tackling large-scale projects.</span></span>

<span data-ttu-id="44f89-109">让我们了解各个选项，以及每个选项的最具吸引力的方案。</span><span class="sxs-lookup"><span data-stu-id="44f89-109">Let's examine the various options and the most compelling scenarios for each one.</span></span>

## <a name="azure-services-with-r-language-support"></a><span data-ttu-id="44f89-110">支持 R 语言的 Azure 服务</span><span class="sxs-lookup"><span data-stu-id="44f89-110">Azure services with R language support</span></span>

<span data-ttu-id="44f89-111">本文介绍支持 R 语言的以下 Azure 服务：</span><span class="sxs-lookup"><span data-stu-id="44f89-111">This article covers the following Azure services that support the R language:</span></span>

|<span data-ttu-id="44f89-112">服务</span><span class="sxs-lookup"><span data-stu-id="44f89-112">Service</span></span>                                                          |<span data-ttu-id="44f89-113">描述</span><span class="sxs-lookup"><span data-stu-id="44f89-113">Description</span></span>                                                                       |
|-----------------------------------------------------------------|----------------------------------------------------------------------------------|
|[<span data-ttu-id="44f89-114">数据科学虚拟机</span><span class="sxs-lookup"><span data-stu-id="44f89-114">Data Science Virtual Machine</span></span>](#data-science-virtual-machine)    |<span data-ttu-id="44f89-115">用作数据科学工作站或自定义计算目标的自定义 VM</span><span class="sxs-lookup"><span data-stu-id="44f89-115">a customized VM to use as a data science workstation or as a custom compute target</span></span>|
|[<span data-ttu-id="44f89-116">ML Services on HDInsight</span><span class="sxs-lookup"><span data-stu-id="44f89-116">ML Services on HDInsight</span></span>](#ml-services-on-hdinsight)            |<span data-ttu-id="44f89-117">基于群集的系统，用于对跨多个节点的大型数据集运行 R 分析</span><span class="sxs-lookup"><span data-stu-id="44f89-117">cluster-based system for running R analyses on large datasets across many nodes</span></span>   |
|[<span data-ttu-id="44f89-118">Azure Databricks</span><span class="sxs-lookup"><span data-stu-id="44f89-118">Azure Databricks</span></span>](#azure-databricks)                            |<span data-ttu-id="44f89-119">支持 R 和其他语言的协作型 Spark 环境</span><span class="sxs-lookup"><span data-stu-id="44f89-119">collaborative Spark environment that supports R and other languages</span></span>               |
|[<span data-ttu-id="44f89-120">Azure 机器学习工作室</span><span class="sxs-lookup"><span data-stu-id="44f89-120">Azure Machine Learning Studio</span></span>](#azure-machine-learning-studio)  |<span data-ttu-id="44f89-121">在 Azure 的机器学习试验中运行自定义 R 脚本</span><span class="sxs-lookup"><span data-stu-id="44f89-121">run custom R scripts in Azure's machine learning experiments</span></span>                      |
|[<span data-ttu-id="44f89-122">Azure 批处理</span><span class="sxs-lookup"><span data-stu-id="44f89-122">Azure Batch</span></span>](#azure-batch)                                      |<span data-ttu-id="44f89-123">提供各种选项用于以经济节省的方式对群集中的多个节点运行 R 代码</span><span class="sxs-lookup"><span data-stu-id="44f89-123">offers a variety options for economically running R code across many nodes in a cluster</span></span>|
|[<span data-ttu-id="44f89-124">Azure Notebook</span><span class="sxs-lookup"><span data-stu-id="44f89-124">Azure Notebooks</span></span>](#azure-notebooks)                              |<span data-ttu-id="44f89-125">Jupyter Notebook 的基于云的免费版本</span><span class="sxs-lookup"><span data-stu-id="44f89-125">a no-cost cloud-based version of Jupyter notebooks</span></span>                  |
|[<span data-ttu-id="44f89-126">Azure SQL 数据库</span><span class="sxs-lookup"><span data-stu-id="44f89-126">Azure SQL Database</span></span>](#azure-sql-database)                        |<span data-ttu-id="44f89-127">在 SQL Server 数据库引擎内部运行 R 脚本</span><span class="sxs-lookup"><span data-stu-id="44f89-127">run R scripts inside of the SQL Server database engine</span></span>                            |

## <a name="data-science-virtual-machine"></a><span data-ttu-id="44f89-128">数据科学虚拟机</span><span class="sxs-lookup"><span data-stu-id="44f89-128">Data Science Virtual Machine</span></span>

<span data-ttu-id="44f89-129">[Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) (DSVM) 是专为开展数据科学构建的 Microsoft Azure 云平台上的自定义 VM 映像。</span><span class="sxs-lookup"><span data-stu-id="44f89-129">The [Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) (DSVM) is a customized VM image on Microsoft’s Azure cloud platform built specifically for doing data science.</span></span> <span data-ttu-id="44f89-130">其中包含许多热门的数据科学工具，包括：</span><span class="sxs-lookup"><span data-stu-id="44f89-130">It has many popular data science tools, including:</span></span>

* [<span data-ttu-id="44f89-131">Microsoft R Open</span><span class="sxs-lookup"><span data-stu-id="44f89-131">Microsoft R Open</span></span>](https://mran.microsoft.com/open/)
* [<span data-ttu-id="44f89-132">Microsoft 机器学习服务器</span><span class="sxs-lookup"><span data-stu-id="44f89-132">Microsoft Machine Learning Server</span></span>](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server)
* [<span data-ttu-id="44f89-133">RStudio Desktop</span><span class="sxs-lookup"><span data-stu-id="44f89-133">RStudio Desktop</span></span>](https://www.rstudio.com/products/rstudio/#Desktop)
* [<span data-ttu-id="44f89-134">RStudio Server</span><span class="sxs-lookup"><span data-stu-id="44f89-134">RStudio Server</span></span>](https://www.rstudio.com/products/rstudio/#Server)

<span data-ttu-id="44f89-135">可以在 DSVM 上预配 Windows 或 Linux 操作系统。</span><span class="sxs-lookup"><span data-stu-id="44f89-135">The DSVM can be provisioned with either Windows or Linux as the operating system.</span></span>  <span data-ttu-id="44f89-136">可通过两种方式使用 DSVM：用作交互式工作站，或用作自定义群集的计算平台。</span><span class="sxs-lookup"><span data-stu-id="44f89-136">You can use the DSVM in two different ways:  as an interactive workstation or as a compute platform for a custom cluster.</span></span>

### <a name="as-a-workstation"></a><span data-ttu-id="44f89-137">用作工作站</span><span class="sxs-lookup"><span data-stu-id="44f89-137">As a workstation</span></span>

<span data-ttu-id="44f89-138">若要在云中快速轻松地开始使用 R，则这是最佳选项。</span><span class="sxs-lookup"><span data-stu-id="44f89-138">If you want to get started with R in the cloud quickly and easily, this is your best bet.</span></span>  <span data-ttu-id="44f89-139">在本地工作站上用过 R 的任何人都会熟悉该环境。</span><span class="sxs-lookup"><span data-stu-id="44f89-139">The environment will be familiar to anyone who has worked with R on a local workstation.</span></span>  <span data-ttu-id="44f89-140">但是，R 环境不是使用本地资源，而是在云中的 VM 上运行。</span><span class="sxs-lookup"><span data-stu-id="44f89-140">However, instead of using local resources, the R environment runs on a VM in the cloud.</span></span>  <span data-ttu-id="44f89-141">如果数据已存储在 Azure 中，则此环境还能带来另一种优势：R 脚本可以在“更靠近数据”的位置运行。</span><span class="sxs-lookup"><span data-stu-id="44f89-141">If your data is already stored in Azure, this has the added benefit of allowing your R scripts to run "closer to the data."</span></span> <span data-ttu-id="44f89-142">无需通过 Internet 传输数据，可以通过 Azure 的内部网络访问数据，因此访问速度要快得多。</span><span class="sxs-lookup"><span data-stu-id="44f89-142">Instead of transferring the data across the Internet, the data can be accessed over Azure's internal network, which provides much faster access times.</span></span>

<span data-ttu-id="44f89-143">DSVM 可能对小型 R 开发人员团队特别有用。</span><span class="sxs-lookup"><span data-stu-id="44f89-143">The DSVM can be particularly useful to small teams of R developers.</span></span>  <span data-ttu-id="44f89-144">无需为每个开发人员投资购买高配的工作站并要求团队成员同步他们使用的各个软件包版本，每个开发人员可以根据需要运转 DSVM 的实例。</span><span class="sxs-lookup"><span data-stu-id="44f89-144">Instead of investing in powerful workstations for each developer and requiring team members to synchronize on which versions of the various software packages they will use, each developer can spin up an instance of the DSVM whenever needed.</span></span>

### <a name="as-a-compute-platform"></a><span data-ttu-id="44f89-145">用作计算平台</span><span class="sxs-lookup"><span data-stu-id="44f89-145">As a compute platform</span></span>

<span data-ttu-id="44f89-146">除了用作工作站以外，DSVM 还可用作 R 项目的弹性可缩放计算平台。</span><span class="sxs-lookup"><span data-stu-id="44f89-146">In addition to being used as a workstation, the DSVM is also used as an elastically scalable compute platform for R projects.</span></span>  <span data-ttu-id="44f89-147">使用[ `AzureDSVM` ](https://github.com/Azure/AzureDSVM) R 包，您可以以编程方式控制创建和删除 DSVM 实例。</span><span class="sxs-lookup"><span data-stu-id="44f89-147">Using the [`AzureDSVM`](https://github.com/Azure/AzureDSVM) R package, you can programmatically control the creation and deletion of DSVM instances.</span></span>  <span data-ttu-id="44f89-148">可将实例组建成群集，并部署要在云中执行的分布式分析。</span><span class="sxs-lookup"><span data-stu-id="44f89-148">You can form the instances into a cluster and deploy a distributed analysis to be performed in the cloud.</span></span>  <span data-ttu-id="44f89-149">可以通过本地工作站上运行的 R 代码控制整个过程。</span><span class="sxs-lookup"><span data-stu-id="44f89-149">This entire process can be controlled by R code running on your local workstation.</span></span>

<span data-ttu-id="44f89-150">若要了解有关 DSVM 的详细信息，请参阅[适用于 Linux 和 Windows 的 Azure 数据科学虚拟机简介](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview)。</span><span class="sxs-lookup"><span data-stu-id="44f89-150">To learn more about the DSVM, see [Introduction to Azure Data Science Virtual Machine for Linux and Windows](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview).</span></span>

## <a name="ml-services-on-hdinsight"></a><span data-ttu-id="44f89-151">ML Services on HDInsight</span><span class="sxs-lookup"><span data-stu-id="44f89-151">ML Services on HDInsight</span></span>

<span data-ttu-id="44f89-152">[Microsoft ML Services](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-overview) 可让数据科学家、统计师和 R 程序员按需访问 HDInsight 上可缩放的分布式分析方法。</span><span class="sxs-lookup"><span data-stu-id="44f89-152">[Microsoft ML Services](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-overview) provide data scientists, statisticians, and R programmers with on-demand access to scalable, distributed methods of analytics on HDInsight.</span></span>  <span data-ttu-id="44f89-153">此解决方案提供最新的功能，可针对载入 Azure Blob 或 Data Lake Storage 的几乎任何大小的数据集执行基于 R 的分析。</span><span class="sxs-lookup"><span data-stu-id="44f89-153">This solution provides the latest capabilities for R-based analytics on datasets of virtually any size, loaded to either Azure Blob or Data Lake storage.</span></span>

<span data-ttu-id="44f89-154">这是一个企业级的解决方案，允许在整个群集中缩放 R 代码。</span><span class="sxs-lookup"><span data-stu-id="44f89-154">This is an enterprise-grade solution that allows you to scale your R code across a cluster.</span></span>  <span data-ttu-id="44f89-155">通过利用 Microsoft 的中的函数 [`RevoScaleR`](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler)</span><span class="sxs-lookup"><span data-stu-id="44f89-155">By leveraging functions in Microsoft's [`RevoScaleR`](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler)</span></span>
<span data-ttu-id="44f89-156">包中，你在 HDInsight 上的 R 脚本可以数据处理函数并行运行跨多个节点在群集中。</span><span class="sxs-lookup"><span data-stu-id="44f89-156">package, your R scripts on HDInsight can run data processing functions in parallel across many nodes in a cluster.</span></span>  <span data-ttu-id="44f89-157">这样，R 便可以使用工作站上运行的单线程 R，以远超寻常的规模处理数据。</span><span class="sxs-lookup"><span data-stu-id="44f89-157">This allows R to crunch data on a much larger scale than is possible with single-threaded R running on a workstation.</span></span>

<span data-ttu-id="44f89-158">这种大规模处理能力使得 ML Services on HDInsight 成了需要处理巨量数据集的 R 开发人员的极佳选项。</span><span class="sxs-lookup"><span data-stu-id="44f89-158">This ability to scale makes ML Services on HDInsight a great option for R developers with massive data sets.</span></span>  <span data-ttu-id="44f89-159">它提供一个灵活、可缩放的平台用于在云中运行 R 脚本。</span><span class="sxs-lookup"><span data-stu-id="44f89-159">It provides a flexible and scalable platform for running your R scripts in the cloud.</span></span>

<span data-ttu-id="44f89-160">创建机器学习服务群集的演练，请参阅[开始使用 Azure HDInsight 上的机器学习服务](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-get-started)。</span><span class="sxs-lookup"><span data-stu-id="44f89-160">For a walk-through on creating an ML Services cluster, see [Get started with ML Services on Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-get-started).</span></span>

## <a name="azure-databricks"></a><span data-ttu-id="44f89-161">Azure Databricks</span><span class="sxs-lookup"><span data-stu-id="44f89-161">Azure Databricks</span></span>

<span data-ttu-id="44f89-162">[Azure Databricks](https://azure.microsoft.com/services/databricks/) 是基于 Apache Spark 的分析平台，已针对 Microsoft Azure 云服务平台进行优化。</span><span class="sxs-lookup"><span data-stu-id="44f89-162">[Azure Databricks](https://azure.microsoft.com/services/databricks/) is an Apache Spark-based analytics platform optimized for the Microsoft Azure cloud services platform.</span></span>  <span data-ttu-id="44f89-163">我们与 Apache Spark 的创建者一起设计了 Databricks，并将其与 Azure 集成以提供一键式安装程序、简化的工作流程以及交互式工作区，从而使数据科学家、数据工程师和业务分析员之间可以进行合作。</span><span class="sxs-lookup"><span data-stu-id="44f89-163">Designed with the founders of Apache Spark, Databricks is integrated with Azure to provide one-click setup, streamlined workflows, and an interactive workspace that enables collaboration between data scientists, data engineers, and business analysts.</span></span>

<span data-ttu-id="44f89-164">Databricks 中的协作由平台的 Notebook 系统启用。</span><span class="sxs-lookup"><span data-stu-id="44f89-164">The collaboration in Databricks is enabled by the platform's notebook system.</span></span>  <span data-ttu-id="44f89-165">用户可以创建、编辑 Notebook 并与系统的其他用户共享。</span><span class="sxs-lookup"><span data-stu-id="44f89-165">Users can create, share, and edit notebooks with other users of the systems.</span></span>  <span data-ttu-id="44f89-166">用户可以使用这些 Notebook 编写针对 Databricks 环境中托管的 Spark 群集执行的代码。</span><span class="sxs-lookup"><span data-stu-id="44f89-166">These notebooks allow users to write code that executes against Spark clusters managed in the Databricks environment.</span></span>  <span data-ttu-id="44f89-167">这些 Notebook 完全支持 R，并可让用户通过 `SparkR` 和 `sparklyr` 包访问 Spark。</span><span class="sxs-lookup"><span data-stu-id="44f89-167">These notebooks fully support R and give users access to Spark through both the `SparkR` and `sparklyr` packages.</span></span>

<span data-ttu-id="44f89-168">由于 Databricks 构建在 Spark 基础之上并且侧重于协作，该平台往往由共同解决大型数据集复杂分析的数据科学家团队使用。</span><span class="sxs-lookup"><span data-stu-id="44f89-168">Since Databricks is built on Spark and has a strong focus on collaboration, the platform is often used by teams of data scientists that work together on complex analyses of large data sets.</span></span>  <span data-ttu-id="44f89-169">由于 Databricks 中的 Notebook 除了支持 R 以外还支持其他语言，因此，它对于在主要工作中使用不同语言的分析师团队非常有用。</span><span class="sxs-lookup"><span data-stu-id="44f89-169">Because the notebooks in Databricks support other languages in addition to R, it is especially useful for teams where analysts use different languages for their primary work.</span></span>

<span data-ttu-id="44f89-170">文章[什么是 Azure Databricks？](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks)可以提供有关平台和帮助您入门的更多详细信息。</span><span class="sxs-lookup"><span data-stu-id="44f89-170">The article [What is Azure Databricks?](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks) can provide more details about the platform and help you get started.</span></span>

## <a name="azure-machine-learning-studio"></a><span data-ttu-id="44f89-171">Azure 机器学习工作室</span><span class="sxs-lookup"><span data-stu-id="44f89-171">Azure Machine Learning Studio</span></span>

<span data-ttu-id="44f89-172">[Azure 机器学习工作室](https://azure.microsoft.com/services/machine-learning-studio/)是一个协作型拖放式工具，可用于在云中构建、测试和部署预测分析解决方案。</span><span class="sxs-lookup"><span data-stu-id="44f89-172">[Azure Machine Learning Studio](https://azure.microsoft.com/services/machine-learning-studio/) is a collaborative, drag-and-drop tool you can use to build, test, and deploy predictive analytics solutions in the cloud.</span></span>  <span data-ttu-id="44f89-173">越来越多的数据科学家正在使用它来创建和部署机器学习模型，而无需编写大量的代码。</span><span class="sxs-lookup"><span data-stu-id="44f89-173">It enables emerging data scientists to create and deploy machine learning models without the need to write much code.</span></span>

<span data-ttu-id="44f89-174">Azure 机器学习工作室支持 R 和 Python。</span><span class="sxs-lookup"><span data-stu-id="44f89-174">Azure Machine Learning Studio supports both R and Python.</span></span>  <span data-ttu-id="44f89-175">两种方式，可以通过 Azure 机器学习工作室中使用 R。</span><span class="sxs-lookup"><span data-stu-id="44f89-175">You can use R with Azure Machine Learning Studio in two ways.</span></span>

### <a name="custom-r-scripts-in-your-experiments"></a><span data-ttu-id="44f89-176">试验中的自定义 R 脚本</span><span class="sxs-lookup"><span data-stu-id="44f89-176">Custom R scripts in your experiments</span></span>

<span data-ttu-id="44f89-177">首先，可以通过编写自定义 R 脚本来扩展机器学习工作室的数据处理和机器学习功能。</span><span class="sxs-lookup"><span data-stu-id="44f89-177">First, you can extend the data manipulation and machine learning capabilities of ML Studio by writing custom R scripts.</span></span>
<span data-ttu-id="44f89-178">机器学习工作室包含各种模块用于准备和分析数据，但还比不上 R 等成熟语言的功能。因此，当提供的模块无法满足需求时，该服务允许你引入自己的自定义 R 脚本。</span><span class="sxs-lookup"><span data-stu-id="44f89-178">Although ML Studio includes a wide variety of modules for preparing and analyzing data, it cannot match the capabilities of a mature language like R.  Therefore, the service was designed to allow you to introduce your own custom R scripts in cases where the provided modules do not meet your needs.</span></span>

<span data-ttu-id="44f89-179">若要利用此功能，请将“执行 R 脚本”模块拖到试验中。</span><span class="sxs-lookup"><span data-stu-id="44f89-179">To leverage this capability, drag and drop an "Execute R Script" module into your experiment.</span></span>  <span data-ttu-id="44f89-180">然后，使用“属性”窗格中的代码编辑器编写新的 R 脚本，或粘贴现有脚本。</span><span class="sxs-lookup"><span data-stu-id="44f89-180">Then use the code editor in the "Properties" pane to write a new R script or paste an existing script.</span></span>  <span data-ttu-id="44f89-181">在脚本中，可以引用外部 R 包。</span><span class="sxs-lookup"><span data-stu-id="44f89-181">Within the script, you can reference external R packages.</span></span>  <span data-ttu-id="44f89-182">若要处理的数据或训练复杂不是标准的 Azure 机器学习工作室模型库的一部分的机器学习模型，可以使用该脚本。</span><span class="sxs-lookup"><span data-stu-id="44f89-182">You can use the script to manipulate data or to train complex ML models that are not part of the standard Azure Machine Learning Studio model library.</span></span>

<span data-ttu-id="44f89-183">有关在机器学习工作室试验中使用 R 的全面介绍，请参阅[R 编程语言中 Azure 机器学习工作室入门](https://docs.microsoft.com/azure/machine-learning/studio/r-quickstart)。</span><span class="sxs-lookup"><span data-stu-id="44f89-183">For a thorough introduction on using R within ML Studio experiments, check out [Getting started with the R programming language in Azure Machine Learning Studio](https://docs.microsoft.com/azure/machine-learning/studio/r-quickstart).</span></span>

### <a name="create-manage-and-deploy-experiments-from-your-local-r-environment"></a><span data-ttu-id="44f89-184">在本地 R 环境中创建、管理和部署试验</span><span class="sxs-lookup"><span data-stu-id="44f89-184">Create, manage, and deploy experiments from your local R environment</span></span>

<span data-ttu-id="44f89-185">您可以通过 Azure 机器学习工作室中使用 R 的其他方法是使用[ `AzureML` ](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html)包来监视和控制与 R 编程环境对试验过程。</span><span class="sxs-lookup"><span data-stu-id="44f89-185">The other way that you can use R with Azure Machine Learning Studio is to use the [`AzureML`](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) package to monitor and control the experimentation process with the R programming environment.</span></span>  <span data-ttu-id="44f89-186">此包，由 Microsoft 维护，可以上传和下载数据集与 Azure 机器学习工作室中，以询问试验，发布 R 函数作为 web 服务，运行 R 数据并通过现有 web 服务检索输出。</span><span class="sxs-lookup"><span data-stu-id="44f89-186">This package, which is maintained by Microsoft, allows you to upload and download datasets to and from Azure Machine Learning Studio, to interrogate experiments, to publish R functions as web services, and to run R data through existing web services and retrieve the output.</span></span>

<span data-ttu-id="44f89-187">此包，可以更方便地为你的 R 代码作为可缩放的部署平台使用 Azure 机器学习工作室。</span><span class="sxs-lookup"><span data-stu-id="44f89-187">This package makes it much easier to use Azure Machine Learning Studio as a scalable deployment platform for your R code.</span></span>  <span data-ttu-id="44f89-188">无需在 UI 中不断地单击和拖放，而可以使用熟知的工具将整个部署过程自动化。</span><span class="sxs-lookup"><span data-stu-id="44f89-188">Instead of clicking and dragging in the UI, you can automate the entire deployment process using tools you already know.</span></span>

## <a name="azure-batch"></a><span data-ttu-id="44f89-189">Azure 批处理</span><span class="sxs-lookup"><span data-stu-id="44f89-189">Azure Batch</span></span>

<span data-ttu-id="44f89-190">对于大规模 R 作业，可以使用 [Azure Batch](https://azure.microsoft.com/services/batch/)。</span><span class="sxs-lookup"><span data-stu-id="44f89-190">For large-scale R jobs, you can use [Azure Batch](https://azure.microsoft.com/services/batch/).</span></span>  <span data-ttu-id="44f89-191">此服务提供云规模的作业计划和计算管理，可让你缩放跨数十、数百甚至数千个虚拟机的 R 工作负荷。</span><span class="sxs-lookup"><span data-stu-id="44f89-191">This service provides cloud-scale job scheduling and compute management so you can scale your R workload across tens, hundreds, or thousands of virtual machines.</span></span>  <span data-ttu-id="44f89-192">由于它是一个通用化的计算平台，在 Azure Batch 上运行 R 作业的选项有很多。</span><span class="sxs-lookup"><span data-stu-id="44f89-192">Since it is a generalized computing platform, there a few options for running R jobs on Azure Batch.</span></span>

<span data-ttu-id="44f89-193">一种选择是使用 Microsoft [ `doAzureParallel` ](https://github.com/Azure/doAzureParallel)包。</span><span class="sxs-lookup"><span data-stu-id="44f89-193">One option is to use Microsoft's [`doAzureParallel`](https://github.com/Azure/doAzureParallel) package.</span></span>  <span data-ttu-id="44f89-194">此 R 包是 `foreach` 包的并行后端。</span><span class="sxs-lookup"><span data-stu-id="44f89-194">This R package is a parallel backend for the `foreach` package.</span></span>  <span data-ttu-id="44f89-195">使用此包可在 Azure Batch 群集中的节点上并行运行 `foreach` 循环的每个迭代。</span><span class="sxs-lookup"><span data-stu-id="44f89-195">It allows each iteration of the `foreach` loop to run in parallel on a node within the Azure Batch cluster.</span></span>  <span data-ttu-id="44f89-196">有关包的介绍，请参阅博客文章[doAzureParallel:充分利用 Azure 的灵活计算直接从 R 会话](https://azure.microsoft.com/blog/doazureparallel/)。</span><span class="sxs-lookup"><span data-stu-id="44f89-196">For an introduction to the package, see the blog post [doAzureParallel: Take advantage of Azure’s flexible compute directly from your R session](https://azure.microsoft.com/blog/doazureparallel/).</span></span>

<span data-ttu-id="44f89-197">在 Azure Batch 中运行 R 脚本的另一个选项是在 Azure 门户中使用“RScript.exe”将代码捆绑为 Batch 应用。</span><span class="sxs-lookup"><span data-stu-id="44f89-197">Another option for running an R script in Azure Batch is to bundle your code with "RScript.exe" as a Batch App in the Azure portal.</span></span>  <span data-ttu-id="44f89-198">有关详细的演练，请查阅[Azure Batch 上的 R 工作负荷](https://azure.microsoft.com/blog/r-workloads-on-azure-batch/)。</span><span class="sxs-lookup"><span data-stu-id="44f89-198">For a detailed walk-through, consult [R Workloads on Azure Batch](https://azure.microsoft.com/blog/r-workloads-on-azure-batch/).</span></span>

<span data-ttu-id="44f89-199">第三个选项是使用 [Azure 分布式数据工程工具包](https://github.com/Azure/aztk) (AZTK)。该工具包可让你使用 Azure Batch 中的 Docker 容器预配按需 Spark 群集。</span><span class="sxs-lookup"><span data-stu-id="44f89-199">A third option is to use the [Azure Distributed Data Engineering Toolkit](https://github.com/Azure/aztk) (AZTK), which allows you to provision on-demand Spark clusters using Docker containers in Azure Batch.</span></span>  <span data-ttu-id="44f89-200">这样，便可以经济节省的方式在 Azure 中运行 Spark 作业。</span><span class="sxs-lookup"><span data-stu-id="44f89-200">This provides an economical way to run Spark jobs in Azure.</span></span>  <span data-ttu-id="44f89-201">使用 [SparklyR 和 AZTK](https://github.com/Azure/aztk/wiki/SparklyR-on-Azure-with-AZTK) 可以在云中轻松、经济节省地扩展 R 脚本。</span><span class="sxs-lookup"><span data-stu-id="44f89-201">By using [SparklyR with AZTK](https://github.com/Azure/aztk/wiki/SparklyR-on-Azure-with-AZTK), your R scripts can be scaled out in the cloud easily and economically.</span></span>

## <a name="azure-notebooks"></a><span data-ttu-id="44f89-202">Azure Notebook</span><span class="sxs-lookup"><span data-stu-id="44f89-202">Azure Notebooks</span></span>

<span data-ttu-id="44f89-203">[Azure Notebooks](https://notebooks.azure.com) 是一种成本低、冲突少的服务，适合偏向于使用 Notebook 将代码部署到 Azure 的 R 开发人员。</span><span class="sxs-lookup"><span data-stu-id="44f89-203">[Azure Notebooks](https://notebooks.azure.com) is a low-cost, low-friction method for R developers who prefer working with notebooks to bring their code to Azure.</span></span>  <span data-ttu-id="44f89-204">它是一个免费的服务，面向使用 [Jupyter](https://jupyter.org/)（一个开源项目，可将 markdown 文本信息、可执行代码和图形合并到一个画布上）在浏览器中开发和运行代码的任何用户。</span><span class="sxs-lookup"><span data-stu-id="44f89-204">It is a free service for anyone to develop and run code in their browser using [Jupyter](https://jupyter.org/), which is an open-source project that enables combing markdown prose, executable code, and graphics onto a single canvas.</span></span>

<span data-ttu-id="44f89-205">Azure Notebooks 的免费服务层是适用于小规模项目的可行选项，因为它将每个 Notebook 的处理能力限制为 4 GB 内存和 1 GB 数据集。</span><span class="sxs-lookup"><span data-stu-id="44f89-205">The free service tier of Azure Notebooks is a viable option for small-scale projects, as it limits each notebook's process to 4GB of memory and 1GB data sets.</span></span> <span data-ttu-id="44f89-206">但是，如果需要超出这些限制的计算和数据处理能力，则可以在 Data Science Virtual Machine 实例中运行 Notebook。</span><span class="sxs-lookup"><span data-stu-id="44f89-206">If you need compute and data power beyond these limitations, however, you can run notebooks in a Data Science Virtual Machine instance.</span></span> <span data-ttu-id="44f89-207">有关详细信息，请参阅[管理和配置 Azure Notebooks 项目 - 计算层](/azure/notebooks/configure-manage-azure-notebooks-projects#compute-tier)。</span><span class="sxs-lookup"><span data-stu-id="44f89-207">For more information, see [Manage and configure Azure Notebooks projects - Compute tier](/azure/notebooks/configure-manage-azure-notebooks-projects#compute-tier).</span></span>

## <a name="azure-sql-database"></a><span data-ttu-id="44f89-208">Azure SQL 数据库</span><span class="sxs-lookup"><span data-stu-id="44f89-208">Azure SQL Database</span></span>

<span data-ttu-id="44f89-209">[Azure SQL 数据库](https://azure.microsoft.com/services/sql-database/)是 Microsoft 提供的完全托管式的智能关系型云数据库服务。</span><span class="sxs-lookup"><span data-stu-id="44f89-209">[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) is Microsoft's intelligent, fully managed relational cloud database service.</span></span>  <span data-ttu-id="44f89-210">它可以让你使用 SQL Server 的完整功能，省去了设置基础结构的麻烦。</span><span class="sxs-lookup"><span data-stu-id="44f89-210">It allows you to use the full power of SQL Server without any hassle of setting up the infrastructure.</span></span>  <span data-ttu-id="44f89-211">这包括[SQL Server 中机器学习服务](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning?view=sql-server-2017)，这是 SQL 的较新功能之一。</span><span class="sxs-lookup"><span data-stu-id="44f89-211">This includes [Machine Learning Services in SQL Server](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning?view=sql-server-2017), which is one of the more recent additions to SQL.</span></span>

<span data-ttu-id="44f89-212">此功能提供嵌入式预测分析和数据科学引擎，该引擎可将 SQL Server 数据库中的 R 代码作为存储过程、包含 R 语句的 T-SQL 脚本或包含 T-SQL 的 R 代码来执行。</span><span class="sxs-lookup"><span data-stu-id="44f89-212">This feature offers an embedded, predictive analytics and data science engine that can execute R code within a SQL Server database as stored procedures, as T-SQL scripts containing R statements, or as R code containing T-SQL.</span></span>  <span data-ttu-id="44f89-213">无需从数据库中提取数据并将其载入 R 环境，而可以直接将 R 代码载入数据库，使其连接数据一起运行。</span><span class="sxs-lookup"><span data-stu-id="44f89-213">Instead of extracting data from the database and loading it into the R environment, you load your R code directly into the database and let it run right alongside the data.</span></span>

<span data-ttu-id="44f89-214">从 2016 年开始，机器学习服务已划归到本地 SQL Server，是 Azure SQL 数据库的相对较新的功能。</span><span class="sxs-lookup"><span data-stu-id="44f89-214">While Machine Learning Services has been part of on-premises SQL Server since 2016, it is relatively new to Azure SQL Database.</span></span>  <span data-ttu-id="44f89-215">它目前以[受限预览版](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#azure-sql-database-roadmap)的形式提供，同时在不断改善。</span><span class="sxs-lookup"><span data-stu-id="44f89-215">It is currently in [limited preview](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#azure-sql-database-roadmap) but will continue to evolve.</span></span>


### <a name="next-steps"></a><span data-ttu-id="44f89-216">后续步骤</span><span class="sxs-lookup"><span data-stu-id="44f89-216">Next steps</span></span>

* [<span data-ttu-id="44f89-217">使用 mrsdeploy 在 Azure 上运行 R 代码</span><span class="sxs-lookup"><span data-stu-id="44f89-217">Running your R code on Azure with mrsdeploy</span></span>](https://blog.revolutionanalytics.com/2017/03/running-your-r-code-azure.html)
* [<span data-ttu-id="44f89-218">机器学习在云中的服务器</span><span class="sxs-lookup"><span data-stu-id="44f89-218">Machine Learning Server in the Cloud</span></span>](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-in-the-cloud)
* [<span data-ttu-id="44f89-219">机器学习服务器和 Microsoft R 的其他资源</span><span class="sxs-lookup"><span data-stu-id="44f89-219">Additional Resources for Machine Learning Server and Microsoft R</span></span>](https://docs.microsoft.com/machine-learning-server/resources-more)
* <span data-ttu-id="44f89-220">[R on Azure](https://github.com/yueguoguo/r-on-azure) - 概述在 Azure 中使用 R 所需的包、工具和相关案例研究</span><span class="sxs-lookup"><span data-stu-id="44f89-220">[R on Azure](https://github.com/yueguoguo/r-on-azure) - an overview of packages, tools, and case studies for using R with Azure</span></span>

---

<sub><span data-ttu-id="44f89-221">R 徽标&copy;2016年 R Foundation 和使用的条款[Creative Commons Attribution-sharealike 4.0 国际许可证](https://creativecommons.org/licenses/by-sa/4.0/)。</span><span class="sxs-lookup"><span data-stu-id="44f89-221">The R logo is &copy; 2016 The R Foundation and is used under the terms of the [Creative Commons Attribution-ShareAlike 4.0 International license](https://creativecommons.org/licenses/by-sa/4.0/).</span></span></sub>