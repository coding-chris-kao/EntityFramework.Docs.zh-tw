---
title: 設計階段 DbCoNtext 建立-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f44f0648678af5a70e5171d69692bde1c1d5e0eb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416739"
---
# <a name="design-time-dbcontext-creation"></a>設計階段 DbContext 建立

某些 EF Core 工具命令（例如，[遷移][1]命令）需要在設計階段建立衍生的 `DbContext` 實例，才能收集應用程式實體類型的詳細資料，以及它們如何對應到資料庫架構。 在大部分的情況下，最好是以類似于[在執行時間設定][2]它的方式來設定建立 `DbContext`。

工具有各種方式可嘗試建立 `DbContext`：

## <a name="from-application-services"></a>從應用程式服務

如果您的啟始專案使用[ASP.NET Core Web 主機][3]或[.Net Core 泛型主機][4]，則工具會嘗試從應用程式的服務提供者取得 DbCoNtext 物件。

這些工具會先嘗試叫用 `Program.CreateHostBuilder()`、呼叫 `Build()`，然後存取 `Services` 屬性，藉以取得服務提供者。

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/ApplicationService.cs)]

> [!NOTE]
> 當您建立新的 ASP.NET Core 應用程式時，預設會包含此攔截。

`DbContext` 本身和其任何函式中的任何相依性，都必須在應用程式的服務提供者中註冊為服務。 在[接受 `DbContextOptions<TContext>` 實例做為引數][5]並使用[`AddDbContext<TContext>` 方法][6]的 `DbContext` 上，可以輕鬆達成此目的。

## <a name="using-a-constructor-with-no-parameters"></a>使用不含參數的函式

如果無法從應用程式服務提供者取得 DbCoNtext，工具會在專案內尋找衍生的 `DbContext` 型別。 然後，他們會嘗試使用不含參數的函式來建立實例。 如果 `DbContext` 是使用[`OnConfiguring`][7]方法設定的，這可以是預設的函式。

## <a name="from-a-design-time-factory"></a>從設計階段 factory

您也可以藉由執行 `IDesignTimeDbContextFactory<TContext>` 介面，告訴工具如何建立您的 DbCoNtext：如果在與衍生 `DbContext` 相同的專案或在應用程式的啟始專案中找到執行此介面的類別，則工具會略過建立 DbCoNtext 並改用設計階段 factory 的其他方式。

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/BloggingContextFactory.cs)]

> [!NOTE]
> `args` 參數目前未使用。 追蹤從工具指定設計階段引數的能力時發生[問題][8]。

如果您需要在設計階段與執行時間不同的情況下設定 DbCoNtext，則設計階段 factory 特別有用，如果 `DbContext` 的分析程式接受其他參數，而不是在 DI 中註冊，如果您不是使用 DI，或基於某些原因，您不想在 ASP.NET Core 應用程式的 `Main` 類別中有 `BuildWebHost` 方法。

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
