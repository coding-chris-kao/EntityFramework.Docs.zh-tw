---
title: 建立和設定模型 - EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 58be4a45473c6292790da341e360b3340de27be7
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824693"
---
# <a name="creating-and-configuring-a-model"></a>建立和設定模型

Entity Framework 會使用一組慣例，根據您實體類別的圖形建置模型。 您可以指定其他組態來補充及 (或) 覆寫慣例探索到的項目。

本文涵蓋可套用至將目標設為任何資料存放區之模型的組態，以及將目標設為任何關聯式資料庫時可套用的組態。 提供者也可以啟用特定資料存放區專屬的組態。 如需提供者專屬組態的文件，請參閱 [資料庫提供者](../providers/index.md) 一節。

> [!TIP]  
> 您可以在 GitHub 上檢視本文的 [範例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) 。

## <a name="use-fluent-api-to-configure-a-model"></a>使用 Fluent API 設定模型

您可以覆寫衍生內容中的  `OnModelCreating` 方法，並用  `ModelBuilder API` 來設定模型。 這是最強大的組態方法，讓您能夠指定組態而不修改實體類別。 Fluent API 組態具有最高的優先順序，且會覆寫慣例及資料註解。

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

## <a name="use-data-annotations-to-configure-a-model"></a>使用資料註解

您也可以將屬性 (又稱資料註解) 套用到類別及屬性。 資料註解會覆寫慣例，但會受到 Fluent API 組態覆寫。

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]
