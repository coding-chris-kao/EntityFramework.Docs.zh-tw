---
title: 記錄-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: e8adc39ec01ff75112b03446a488df6199cc7041
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416579"
---
# <a name="logging"></a><span data-ttu-id="06920-102">記錄</span><span class="sxs-lookup"><span data-stu-id="06920-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="06920-103">您可以在 GitHub 上檢視此文章的[範例](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="06920-103">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="06920-104">ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="06920-104">ASP.NET Core applications</span></span>

<span data-ttu-id="06920-105">EF Core 在使用 `AddDbContext` 或 `AddDbContextPool` 時，會自動與 ASP.NET Core 的記錄機制整合。</span><span class="sxs-lookup"><span data-stu-id="06920-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="06920-106">因此，使用 ASP.NET Core 時，應該依照[ASP.NET Core 檔](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)中所述設定記錄。</span><span class="sxs-lookup"><span data-stu-id="06920-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="06920-107">其他應用程式</span><span class="sxs-lookup"><span data-stu-id="06920-107">Other applications</span></span>

<span data-ttu-id="06920-108">EF Core 記錄需要 ILoggerFactory，其本身是以一或多個記錄提供者來設定。</span><span class="sxs-lookup"><span data-stu-id="06920-108">EF Core logging requires an ILoggerFactory which is itself configured with one or more logging providers.</span></span> <span data-ttu-id="06920-109">一般提供者隨附于下列套件：</span><span class="sxs-lookup"><span data-stu-id="06920-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="06920-110">[ [Microsoft Extensions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/)]：簡單的主控台記錄器。</span><span class="sxs-lookup"><span data-stu-id="06920-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="06920-111">[Microsoft.extensions.logging.azureappservices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/)：支援 Azure App 服務的「診斷記錄」和「記錄資料流程」功能。</span><span class="sxs-lookup"><span data-stu-id="06920-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="06920-112">[[記錄](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/)檔]：使用 system.servicemodel （）記錄到偵錯工具監視器。</span><span class="sxs-lookup"><span data-stu-id="06920-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="06920-113">[記錄檔]。 [EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/)：記錄到 Windows 事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="06920-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="06920-114">。 [Eventsource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/)：支援 Eventsource/EventListener。</span><span class="sxs-lookup"><span data-stu-id="06920-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="06920-115">[TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/)：使用 `System.Diagnostics.TraceSource.TraceEvent()`記錄至追蹤接聽程式。</span><span class="sxs-lookup"><span data-stu-id="06920-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using `System.Diagnostics.TraceSource.TraceEvent()`.</span></span>

<span data-ttu-id="06920-116">安裝適當的封裝之後，應用程式應該建立 Server.loggerfactory 的單一或全域實例。</span><span class="sxs-lookup"><span data-stu-id="06920-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="06920-117">例如，使用主控台記錄器：</span><span class="sxs-lookup"><span data-stu-id="06920-117">For example, using the console logger:</span></span>

### <a name="version-3x"></a>[<span data-ttu-id="06920-118">3.x 版</span><span class="sxs-lookup"><span data-stu-id="06920-118">Version 3.x</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[<span data-ttu-id="06920-119">2.x 版</span><span class="sxs-lookup"><span data-stu-id="06920-119">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="06920-120">下列程式碼範例會使用在2.2 版中已過時且已在3.0 中取代的 `ConsoleLoggerProvider` 函式。</span><span class="sxs-lookup"><span data-stu-id="06920-120">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="06920-121">使用2.2 時，忽略和隱藏警告是安全的。</span><span class="sxs-lookup"><span data-stu-id="06920-121">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

<span data-ttu-id="06920-122">接著，這個 singleton/global 實例就應該向 `DbContextOptionsBuilder`上的 EF Core 註冊。</span><span class="sxs-lookup"><span data-stu-id="06920-122">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="06920-123">例如：</span><span class="sxs-lookup"><span data-stu-id="06920-123">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="06920-124">應用程式不會為每個內容實例建立新的 ILoggerFactory 實例，這點非常重要。</span><span class="sxs-lookup"><span data-stu-id="06920-124">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="06920-125">這麼做會導致記憶體流失和效能不佳。</span><span class="sxs-lookup"><span data-stu-id="06920-125">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="06920-126">篩選記錄的內容</span><span class="sxs-lookup"><span data-stu-id="06920-126">Filtering what is logged</span></span>

<span data-ttu-id="06920-127">應用程式可以藉由在 ILoggerProvider 上設定篩選準則，來控制所記錄的內容。</span><span class="sxs-lookup"><span data-stu-id="06920-127">The application can control what is logged by configuring a filter on the ILoggerProvider.</span></span> <span data-ttu-id="06920-128">例如：</span><span class="sxs-lookup"><span data-stu-id="06920-128">For example:</span></span>

### <a name="version-3x"></a>[<span data-ttu-id="06920-129">3.x 版</span><span class="sxs-lookup"><span data-stu-id="06920-129">Version 3.x</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[<span data-ttu-id="06920-130">2.x 版</span><span class="sxs-lookup"><span data-stu-id="06920-130">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="06920-131">下列程式碼範例會使用在2.2 版中已過時且已在3.0 中取代的 `ConsoleLoggerProvider` 函式。</span><span class="sxs-lookup"><span data-stu-id="06920-131">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="06920-132">使用2.2 時，忽略和隱藏警告是安全的。</span><span class="sxs-lookup"><span data-stu-id="06920-132">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[]
    {
        new ConsoleLoggerProvider((category, level)
            => category == DbLoggerCategory.Database.Command.Name
               && level == LogLevel.Information, true)
    });
```

***

<span data-ttu-id="06920-133">在此範例中，記錄檔已篩選為只傳回訊息：</span><span class="sxs-lookup"><span data-stu-id="06920-133">In this example, the log is filtered to only return messages:</span></span>

* <span data-ttu-id="06920-134">在 [Microsoft.entityframeworkcore] 類別中</span><span class="sxs-lookup"><span data-stu-id="06920-134">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
* <span data-ttu-id="06920-135">在「資訊」層級</span><span class="sxs-lookup"><span data-stu-id="06920-135">at the 'Information' level</span></span>

<span data-ttu-id="06920-136">針對 EF Core，記錄器分類會在 `DbLoggerCategory` 類別中定義，讓您輕鬆地尋找類別目錄，但這些會解析成簡單的字串。</span><span class="sxs-lookup"><span data-stu-id="06920-136">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="06920-137">如需基礎記錄基礎結構的詳細資訊，請參閱[ASP.NET Core 記錄檔](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)。</span><span class="sxs-lookup"><span data-stu-id="06920-137">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
