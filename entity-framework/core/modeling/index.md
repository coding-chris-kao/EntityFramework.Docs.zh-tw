---
title: 建立和設定模型 - EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 0f44d9684ca5c8435d83085f9038860309bd82a2
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412773"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="4164f-102">建立和設定模型</span><span class="sxs-lookup"><span data-stu-id="4164f-102">Creating and configuring a model</span></span>

<span data-ttu-id="4164f-103">Entity Framework 會使用一組慣例，根據您實體類別的圖形建置模型。</span><span class="sxs-lookup"><span data-stu-id="4164f-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="4164f-104">您可以指定其他組態來補充及 (或) 覆寫慣例探索到的項目。</span><span class="sxs-lookup"><span data-stu-id="4164f-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="4164f-105">本文涵蓋可套用至將目標設為任何資料存放區之模型的組態，以及將目標設為任何關聯式資料庫時可套用的組態。</span><span class="sxs-lookup"><span data-stu-id="4164f-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="4164f-106">提供者也可以啟用特定資料存放區專屬的組態。</span><span class="sxs-lookup"><span data-stu-id="4164f-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="4164f-107">如需提供者專屬組態的文件，請參閱 [資料庫提供者](../providers/index.md) 一節。</span><span class="sxs-lookup"><span data-stu-id="4164f-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="4164f-108">您可以在 GitHub 上檢視本文的 [範例](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) 。</span><span class="sxs-lookup"><span data-stu-id="4164f-108">You can view this article’s [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="4164f-109">使用 Fluent API 設定模型</span><span class="sxs-lookup"><span data-stu-id="4164f-109">Use fluent API to configure a model</span></span>

<span data-ttu-id="4164f-110">您可以覆寫衍生內容中的  `OnModelCreating` 方法，並用  `ModelBuilder API` 來設定模型。</span><span class="sxs-lookup"><span data-stu-id="4164f-110">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="4164f-111">這是最強大的組態方法，讓您能夠指定組態而不修改實體類別。</span><span class="sxs-lookup"><span data-stu-id="4164f-111">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="4164f-112">Fluent API 組態具有最高的優先順序，且會覆寫慣例及資料註解。</span><span class="sxs-lookup"><span data-stu-id="4164f-112">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="4164f-113">使用資料註解</span><span class="sxs-lookup"><span data-stu-id="4164f-113">Use data annotations to configure a model</span></span>

<span data-ttu-id="4164f-114">您也可以將屬性 (又稱資料註解) 套用到類別及屬性。</span><span class="sxs-lookup"><span data-stu-id="4164f-114">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="4164f-115">資料註解會覆寫慣例，但會受到 Fluent API 組態覆寫。</span><span class="sxs-lookup"><span data-stu-id="4164f-115">Data annotations will override conventions, but will be overridden by Fluent API configuration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
