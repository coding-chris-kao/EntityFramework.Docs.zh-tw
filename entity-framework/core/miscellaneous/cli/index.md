---
title: Entity Framework Core 工具參考 - EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 2821263f1e9cd962569e8050b01d704cf858e6c8
ms.sourcegitcommit: 949faaba02e07e44359e77d7935f540af5c32093
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/03/2020
ms.locfileid: "87526482"
---
# <a name="entity-framework-core-tools-reference"></a>Entity Framework Core 工具參考

Entity Framework Core 工具有助於設計階段開發工作。 它們主要用來管理「移轉」以及將資料庫結構描述反向工程以支援 `DbContext` 和實體類型。

* [EF Core 套件管理員主控台工具](powershell.md) 是在 Visual Studio 中的[套件管理器主控台](/nuget/tools/package-manager-console)執行。

* [EF Core .NET 命令列介面 (CLI) 工具](dotnet.md) 是跨平台 [.NET Core CLI 工具](/dotnet/core/tools/)的擴充功能。 這些工具需要 .NET Core SDK 專案 (專案檔中含 `Sdk="Microsoft.NET.Sdk"` 或類似項目的專案)。

這兩個工具都會公開相同的功能。 如果您要在 Visual Studio 中進行開發，則建議使用**套件管理員主控台**工具，因為它們提供更加整合的體驗。

## <a name="next-steps"></a>後續步驟

* [EF Core 套件管理員主控台工具參考](powershell.md)
* [EF Core .NET CLI 工具參考](dotnet.md)
