---
title: Entity Framework 提供者 - EF6
description: Entity Framework 6 中的 Entity Framework 提供者
author: divega
ms.date: 06/27/2018
uid: ef6/fundamentals/providers/index
ms.openlocfilehash: bb573cb0666556de8fb37802f465af97fc40673a
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073145"
---
# <a name="entity-framework-6-providers"></a><span data-ttu-id="66f3f-103">Entity Framework 6 提供者</span><span class="sxs-lookup"><span data-stu-id="66f3f-103">Entity Framework 6 Providers</span></span>
> [!NOTE]
> <span data-ttu-id="66f3f-104">**僅限 EF6 及更新版本** - Entity Framework 6 已引進此頁面中所討論的功能及 API 等等。</span><span class="sxs-lookup"><span data-stu-id="66f3f-104">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="66f3f-105">如果您使用的是較早版本，則不適用部分或全部的資訊。</span><span class="sxs-lookup"><span data-stu-id="66f3f-105">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="66f3f-106">Entity Framework 目前正在開放原始碼授權下進行開發，EF6 和更新版本將不會隨附於 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="66f3f-106">The Entity Framework is now being developed under an open-source license and EF6 and above will not be shipped as part of the .NET Framework.</span></span> <span data-ttu-id="66f3f-107">這有許多優點，但也需要針對 EF6 組件重建 EF 提供者。</span><span class="sxs-lookup"><span data-stu-id="66f3f-107">This has many advantages but also requires that EF providers be rebuilt against the EF6 assemblies.</span></span> <span data-ttu-id="66f3f-108">這表示 EF5 和先前版本的 EF 提供者在重建之前將不適用於 EF6。</span><span class="sxs-lookup"><span data-stu-id="66f3f-108">This means that EF providers for EF5 and below will not work with EF6 until they have been rebuilt.</span></span>

## <a name="which-providers-are-available-for-ef6"></a><span data-ttu-id="66f3f-109">哪些提供者可供 EF6 使用？</span><span class="sxs-lookup"><span data-stu-id="66f3f-109">Which providers are available for EF6?</span></span>

<span data-ttu-id="66f3f-110">我們知道已針對 EF6 重建的提供者包括：</span><span class="sxs-lookup"><span data-stu-id="66f3f-110">Providers we are aware of that have been rebuilt for EF6 include:</span></span>

*   <span data-ttu-id="66f3f-111">**Microsoft SQL Server 提供者**</span><span class="sxs-lookup"><span data-stu-id="66f3f-111">**Microsoft SQL Server provider**</span></span>
    *   <span data-ttu-id="66f3f-112">從 [Entity Framework 開放原始碼程式碼基底](https://github.com/aspnet/EntityFramework6)建置</span><span class="sxs-lookup"><span data-stu-id="66f3f-112">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="66f3f-113">隨附於 [EntityFramework NuGet 套件](https://nuget.org/packages/EntityFramework)</span><span class="sxs-lookup"><span data-stu-id="66f3f-113">Shipped as part of the [EntityFramework NuGet package](https://nuget.org/packages/EntityFramework)</span></span>
*   <span data-ttu-id="66f3f-114">**Microsoft SQL Server Compact Edition 提供者**</span><span class="sxs-lookup"><span data-stu-id="66f3f-114">**Microsoft SQL Server Compact Edition provider**</span></span>
    *   <span data-ttu-id="66f3f-115">從 [Entity Framework 開放原始碼程式碼基底](https://github.com/aspnet/EntityFramework6)建置</span><span class="sxs-lookup"><span data-stu-id="66f3f-115">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="66f3f-116">隨附於 [EntityFramework.SqlServerCompact NuGet 套件](https://nuget.org/packages/EntityFramework.SqlServerCompact)</span><span class="sxs-lookup"><span data-stu-id="66f3f-116">Shipped in the [EntityFramework.SqlServerCompact NuGet package](https://nuget.org/packages/EntityFramework.SqlServerCompact)</span></span>
*   [<span data-ttu-id="66f3f-117">**Devart dotConnect 資料提供者**</span><span class="sxs-lookup"><span data-stu-id="66f3f-117">**Devart dotConnect Data Providers**</span></span>](https://www.devart.com/dotconnect/)
    *   <span data-ttu-id="66f3f-118">有來自 [Devart](https://www.devart.com/) 且適用於各種資料庫的協力廠商提供者，包括 Oracle、MySQL、PostgreSQL、SQLite、Salesforce、DB2 和 SQL Server</span><span class="sxs-lookup"><span data-stu-id="66f3f-118">There are third-party providers from [Devart](https://www.devart.com/) for a variety of databases including Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2, and SQL Server</span></span>
*   [<span data-ttu-id="66f3f-119">**CData Software 提供者**</span><span class="sxs-lookup"><span data-stu-id="66f3f-119">**CData Software providers**</span></span>](https://www.cdata.com/ado/)
    *   <span data-ttu-id="66f3f-120">有來自 [CData Software](https://www.cdata.com/ado/) 且適用於各種資料存放區的協力廠商提供者，包括 Salesforce、 Azure 表格儲存體、MySql 等等</span><span class="sxs-lookup"><span data-stu-id="66f3f-120">There are third-party providers from [CData Software](https://www.cdata.com/ado/) for a variety of data stores including Salesforce, Azure Table Storage, MySql, and many more</span></span>
*   <span data-ttu-id="66f3f-121">**Firebird 提供者**</span><span class="sxs-lookup"><span data-stu-id="66f3f-121">**Firebird provider**</span></span>
    *   <span data-ttu-id="66f3f-122">以 [NuGet 套件](https://www.nuget.org/packages/EntityFramework.Firebird/)的形式提供給您</span><span class="sxs-lookup"><span data-stu-id="66f3f-122">Available as a [NuGet Package](https://www.nuget.org/packages/EntityFramework.Firebird/)</span></span>
*   <span data-ttu-id="66f3f-123">**Visual Fox Pro 提供者**</span><span class="sxs-lookup"><span data-stu-id="66f3f-123">**Visual Fox Pro provider**</span></span>
    *   <span data-ttu-id="66f3f-124">以 [NuGet 套件](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)的形式提供給您</span><span class="sxs-lookup"><span data-stu-id="66f3f-124">Available as a [NuGet package](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span></span>
*   <span data-ttu-id="66f3f-125">**MySQL**</span><span class="sxs-lookup"><span data-stu-id="66f3f-125">**MySQL**</span></span>
    *   [<span data-ttu-id="66f3f-126">MySQL Connector/NET for Entity Framework</span><span class="sxs-lookup"><span data-stu-id="66f3f-126">MySQL Connector/NET for Entity Framework</span></span>](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html)
*   <span data-ttu-id="66f3f-127">**PostgreSQL**</span><span class="sxs-lookup"><span data-stu-id="66f3f-127">**PostgreSQL**</span></span>
    *   <span data-ttu-id="66f3f-128">Npgsql 是以 [NuGet 套件](https://www.nuget.org/packages/EntityFramework6.Npgsql/)的形式提供給您</span><span class="sxs-lookup"><span data-stu-id="66f3f-128">Npgsql is available as a [NuGet package](https://www.nuget.org/packages/EntityFramework6.Npgsql/)</span></span>
*   <span data-ttu-id="66f3f-129">**Oracle**</span><span class="sxs-lookup"><span data-stu-id="66f3f-129">**Oracle**</span></span>
    *   <span data-ttu-id="66f3f-130">ODP.NET 是以 [NuGet 套件](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)的形式提供給您</span><span class="sxs-lookup"><span data-stu-id="66f3f-130">ODP.NET is available as a [NuGet package](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span></span>

<span data-ttu-id="66f3f-131">請注意，這份清單中的包含項目並不表示指定提供者的功能或支援層級，而只是表示已提供適用於 EF6 的組建。</span><span class="sxs-lookup"><span data-stu-id="66f3f-131">Note that inclusion in this list does not indicate the level of functionality or support for a given provider, only that a build for EF6 has been made available.</span></span>

## <a name="registering-ef-providers"></a><span data-ttu-id="66f3f-132">註冊 EF 提供者</span><span class="sxs-lookup"><span data-stu-id="66f3f-132">Registering EF providers</span></span>

<span data-ttu-id="66f3f-133">從 Entity Framework 6 開始，EF 提供者可以使用程式碼架構組態或應用程式的組態檔進行註冊。</span><span class="sxs-lookup"><span data-stu-id="66f3f-133">Starting with Entity Framework 6 EF providers can be registered using either code-based configuration or in the application’s config file.</span></span>

### <a name="config-file-registration"></a><span data-ttu-id="66f3f-134">組態檔註冊</span><span class="sxs-lookup"><span data-stu-id="66f3f-134">Config file registration</span></span>

<span data-ttu-id="66f3f-135">app.config 或 web.config 中的 EF 提供者註冊具有下列格式：</span><span class="sxs-lookup"><span data-stu-id="66f3f-135">Registration of the EF provider in app.config or web.config has the following format:</span></span>


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

<span data-ttu-id="66f3f-136">請注意，通常如果從 NuGet 安裝 EF 提供者，則 NuGet 套件會自動將此註冊新增至設定檔。</span><span class="sxs-lookup"><span data-stu-id="66f3f-136">Note that often if the EF provider is installed from NuGet, then the NuGet package will automatically add this registration to the config file.</span></span> <span data-ttu-id="66f3f-137">如果您將 NuGet 套件安裝到不是應用程式啟始專案的專案中，則可能需要將註冊複製到啟始專案的組態檔。</span><span class="sxs-lookup"><span data-stu-id="66f3f-137">If you install the NuGet package into a project that is not the startup project for your app, then you may need to copy the registration into the config file for your startup project.</span></span>

<span data-ttu-id="66f3f-138">此註冊中的 “invariantName” 是用來識別 ADO.NET 提供者的相同非變異名稱。</span><span class="sxs-lookup"><span data-stu-id="66f3f-138">The “invariantName” in this registration is the same invariant name used to identify an ADO.NET provider.</span></span> <span data-ttu-id="66f3f-139">這可視為 DbProviderFactories 註冊中的 “invariant” 屬性，以及連接字串註冊中的 “providerName” 屬性。</span><span class="sxs-lookup"><span data-stu-id="66f3f-139">This can be found as the “invariant” attribute in a DbProviderFactories registration and as the “providerName” attribute in a connection string registration.</span></span> <span data-ttu-id="66f3f-140">要使用的非變異名稱也應該包含在提供者的文件中。</span><span class="sxs-lookup"><span data-stu-id="66f3f-140">The invariant name to use should also be included in documentation for the provider.</span></span> <span data-ttu-id="66f3f-141">非變異名稱的範例是 “System.Data.SqlClient” (適用於 SQL Server) 和 “System.Data.SqlServerCe.4.0” (適用於 SQL Server Compact)。</span><span class="sxs-lookup"><span data-stu-id="66f3f-141">Examples of invariant names are “System.Data.SqlClient” for SQL Server and “System.Data.SqlServerCe.4.0” for SQL Server Compact.</span></span>

<span data-ttu-id="66f3f-142">此註冊中的 “type” 是衍生自 “System.Data.Entity.Core.Common.DbProviderServices” 之提供者類型的組件限定名稱。</span><span class="sxs-lookup"><span data-stu-id="66f3f-142">The “type” in this registration is the assembly-qualified name of the provider type that derives from “System.Data.Entity.Core.Common.DbProviderServices”.</span></span> <span data-ttu-id="66f3f-143">例如，要用於 SQL Compact 的字串會是 “System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact”。</span><span class="sxs-lookup"><span data-stu-id="66f3f-143">For example, the string to use for SQL Compact is “System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact”.</span></span> <span data-ttu-id="66f3f-144">要在此處使用的類型應該包含在提供者的文件中。</span><span class="sxs-lookup"><span data-stu-id="66f3f-144">The type to use here should be included in documentation for the provider.</span></span>

### <a name="code-based-registration"></a><span data-ttu-id="66f3f-145">程式碼架構註冊</span><span class="sxs-lookup"><span data-stu-id="66f3f-145">Code-based registration</span></span>

<span data-ttu-id="66f3f-146">從 Entity Framework 6 開始，可以在程式碼中指定 EF 的整個應用程式組態。</span><span class="sxs-lookup"><span data-stu-id="66f3f-146">Starting with Entity Framework 6 application-wide configuration for EF can be specified in code.</span></span> <span data-ttu-id="66f3f-147">如需完整的詳細料，請參閱 _[Entity Framework 程式碼架構組態](https://msdn.microsoft.com/data/jj680699)_。</span><span class="sxs-lookup"><span data-stu-id="66f3f-147">For full details see _[Entity Framework Code-Based Configuration](https://msdn.microsoft.com/data/jj680699)_.</span></span> <span data-ttu-id="66f3f-148">使用程式碼架構組態註冊 EF 提供者的一般方式是建立衍生自 System.Data.Entity.DbConfiguration 的新類別，並將它放在與您的 DbContext 類別相同的組件中。</span><span class="sxs-lookup"><span data-stu-id="66f3f-148">The normal way to register an EF provider using code-based configuration is to create a new class that derives from System.Data.Entity.DbConfiguration and place it in the same assembly as your DbContext class.</span></span> <span data-ttu-id="66f3f-149">DbConfiguratio 類別接著應該在其建構函式中註冊提供者。</span><span class="sxs-lookup"><span data-stu-id="66f3f-149">Your DbConfiguration class should then register the provider in its constructor.</span></span> <span data-ttu-id="66f3f-150">例如，若要註冊 SQL Compact 提供者，DbConfiguration 類別看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="66f3f-150">For example, to register the SQL Compact provider the DbConfiguration class looks like this:</span></span>

``` csharp
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetProviderServices(
                SqlCeProviderServices.ProviderInvariantName,
                SqlCeProviderServices.Instance);
        }
    }
```

<span data-ttu-id="66f3f-151">在此程式碼中，“SqlCeProviderServices.ProviderInvariantName” 可讓您方便使用 SQL Server Compact 提供者非變異名稱字串 (“System.Data.SqlServerCe.4.0”)，而 SqlCeProviderServices.Instance 會傳回 SQL Compact EF 提供者的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="66f3f-151">In this code “SqlCeProviderServices.ProviderInvariantName” is a convenience for the SQL Server Compact provider invariant name string (“System.Data.SqlServerCe.4.0”) and SqlCeProviderServices.Instance returns the singleton instance of the SQL Compact EF provider.</span></span>

## <a name="what-if-the-provider-i-need-isnt-available"></a><span data-ttu-id="66f3f-152">如果我需要的提供者無法使用，該怎麼辦？</span><span class="sxs-lookup"><span data-stu-id="66f3f-152">What if the provider I need isn’t available?</span></span>

<span data-ttu-id="66f3f-153">如果該提供者適用於舊版的 EF，則建議您連絡提供者的擁有者，並要求他們建立 EF6 版本。</span><span class="sxs-lookup"><span data-stu-id="66f3f-153">If the provider is available for previous versions of EF, then we encourage you to contact the owner of the provider and ask them to create an EF6 version.</span></span> <span data-ttu-id="66f3f-154">您應該併入 [EF6 提供者模型的文件](xref:ef6/fundamentals/providers/provider-model)參考。</span><span class="sxs-lookup"><span data-stu-id="66f3f-154">You should include a reference to the [documentation for the EF6 provider model](xref:ef6/fundamentals/providers/provider-model).</span></span>

## <a name="can-i-write-a-provider-myself"></a><span data-ttu-id="66f3f-155">我可以自行撰寫提供者嗎？</span><span class="sxs-lookup"><span data-stu-id="66f3f-155">Can I write a provider myself?</span></span>

<span data-ttu-id="66f3f-156">您當然可以自行建立 EF 提供者，但這絕非易事。</span><span class="sxs-lookup"><span data-stu-id="66f3f-156">It is certainly possible to create an EF provider yourself although it should not be considered a trivial undertaking.</span></span> <span data-ttu-id="66f3f-157">有關 EF6 提供者模型的上方連結是一個不錯的起點。</span><span class="sxs-lookup"><span data-stu-id="66f3f-157">The the link above about the EF6 provider model is a good place to start.</span></span> <span data-ttu-id="66f3f-158">您還可能會發現使用 SQL Server 和 SQL CE 提供者的程式碼作為起點或參考很有用，而這些程式碼包含在 [EF 開放原始碼程式碼基底](https://github.com/aspnet/EntityFramework6)中。</span><span class="sxs-lookup"><span data-stu-id="66f3f-158">You may also find it useful to use the code for the SQL Server and SQL CE provider included in the [EF open source codebase](https://github.com/aspnet/EntityFramework6) as a starting point or for reference.</span></span>

<span data-ttu-id="66f3f-159">請注意，從 EF6 開始，EF 提供者不再與基礎 ADO.NET 提供者緊密結合。</span><span class="sxs-lookup"><span data-stu-id="66f3f-159">Note that starting with EF6 the EF provider is less tightly coupled to the underlying ADO.NET provider.</span></span> <span data-ttu-id="66f3f-160">這可讓您更輕鬆地撰寫 EF 提供者，而不必撰寫或包裝 ADO.NET 類別。</span><span class="sxs-lookup"><span data-stu-id="66f3f-160">This makes it easier to write an EF provider without needing to write or wrap the ADO.NET classes.</span></span>
