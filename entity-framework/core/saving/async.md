---
title: 非同步儲存 - EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 0823b86f0579dd3e42f6bd2aebfb433d3cbe00ab
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417639"
---
# <a name="asynchronous-saving"></a>非同步儲存

非同步儲存可避免在將變更寫入至資料庫時阻斷執行緒。 若要避免凍結大型用戶端應用程式的 UI，這非常實用。 非同步作業也可以提高 Web 應用程式中的輸送量，其中可在資料庫作業完成時釋出執行緒，來為其他要求提供服務。 有關詳細資訊,請參閱[C# 中的非同步編程](https://docs.microsoft.com/dotnet/csharp/async)。

> [!WARNING]  
> EF Core 不支援在同一個內容執行個體上執行多個平行作業。 您應該一律等候作業完成，再開始下一項作業。 通常在每個非同步作業上使用 `await` 關鍵字，即可達到此目的。

Entity Framework Core 提供 `DbContext.SaveChangesAsync()` 作為 `DbContext.SaveChanges()`的替代方案。

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
