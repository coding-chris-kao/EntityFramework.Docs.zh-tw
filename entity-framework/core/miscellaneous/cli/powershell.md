---
title: EF Core 工具參考（套件管理員主控台）-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 0676475d46a8d21dee7bd10e25dd273a11e96ac3
ms.sourcegitcommit: 79e460f76b6664e1da5886d102bd97f651d2ffff
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "82538395"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="8fd8c-102">Entity Framework Core 工具參考-Visual Studio 中的套件管理員主控台</span><span class="sxs-lookup"><span data-stu-id="8fd8c-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="8fd8c-103">適用于 Entity Framework Core 的套件管理員主控台（PMC）工具可執行設計階段開發工作。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="8fd8c-104">例如，他們會針對以現有資料庫為基礎的模型建立[遷移](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0)、套用遷移，以及產生程式碼。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="8fd8c-105">命令會使用[套件管理員主控台](/nuget/tools/package-manager-console)在 Visual Studio 內執行。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="8fd8c-106">這些工具會使用 .NET Framework 和 .NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="8fd8c-107">如果您不是使用 Visual Studio，建議改用[EF Core 命令列工具](dotnet.md)。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="8fd8c-108">CLI 工具是跨平臺的，並在命令提示字元中執行。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="8fd8c-109">安裝工具</span><span class="sxs-lookup"><span data-stu-id="8fd8c-109">Installing the tools</span></span>

<span data-ttu-id="8fd8c-110">安裝和更新工具的程式在 ASP.NET Core 2.1 + 和更早版本或其他專案類型之間有所不同。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="8fd8c-111">ASP.NET Core 2.1 版和更新版本</span><span class="sxs-lookup"><span data-stu-id="8fd8c-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="8fd8c-112">這些工具會自動包含在 ASP.NET Core 2.1 + 專案中，因為`Microsoft.EntityFrameworkCore.Tools`此套件包含在[AspNetCore 應用程式中繼套件](/aspnet/core/fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="8fd8c-113">因此，您不需要執行任何動作來安裝工具，但是您必須：</span><span class="sxs-lookup"><span data-stu-id="8fd8c-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>

* <span data-ttu-id="8fd8c-114">在新專案中使用工具之前，請先還原套件。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="8fd8c-115">安裝套件，將工具更新為較新的版本。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="8fd8c-116">若要確定您是取得最新版本的工具，建議您同時執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="8fd8c-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="8fd8c-117">編輯您的 *.csproj*檔案，並加入一行，指定最新版本的[microsoft.entityframeworkcore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/)套件。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="8fd8c-118">例如， *.csproj*檔案可能包含`ItemGroup`如下所示的：</span><span class="sxs-lookup"><span data-stu-id="8fd8c-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="8fd8c-119">當您收到類似下列範例的訊息時，請更新工具：</span><span class="sxs-lookup"><span data-stu-id="8fd8c-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="8fd8c-120">EF Core 工具版本 ' 2.1.1-30846 ' 早于執行時間 ' 2.1.3-rtm-32065 '。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="8fd8c-121">更新工具以取得最新功能和錯誤修正。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="8fd8c-122">若要更新工具：</span><span class="sxs-lookup"><span data-stu-id="8fd8c-122">To update the tools:</span></span>

* <span data-ttu-id="8fd8c-123">安裝最新的 .NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="8fd8c-124">將 Visual Studio 更新為最新版本。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="8fd8c-125">編輯 *.csproj*檔案，使其包含最新工具套件的套件參考，如先前所示。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="8fd8c-126">其他版本和專案類型</span><span class="sxs-lookup"><span data-stu-id="8fd8c-126">Other versions and project types</span></span>

<span data-ttu-id="8fd8c-127">在 [**套件管理員主控台**] 中執行下列命令，以安裝套件管理員主控台工具：</span><span class="sxs-lookup"><span data-stu-id="8fd8c-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="8fd8c-128">在 [**套件管理員主控台**] 中執行下列命令，以更新工具。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="8fd8c-129">驗證安裝</span><span class="sxs-lookup"><span data-stu-id="8fd8c-129">Verify the installation</span></span>

<span data-ttu-id="8fd8c-130">執行下列命令來確認是否已安裝工具：</span><span class="sxs-lookup"><span data-stu-id="8fd8c-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="8fd8c-131">輸出看起來像這樣（它並不會告訴您所使用的工具版本）：</span><span class="sxs-lookup"><span data-stu-id="8fd8c-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

TOPIC
    about_EntityFrameworkCore

SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

<A list of available commands follows, omitted here.>
```

## <a name="using-the-tools"></a><span data-ttu-id="8fd8c-132">使用工具</span><span class="sxs-lookup"><span data-stu-id="8fd8c-132">Using the tools</span></span>

<span data-ttu-id="8fd8c-133">使用工具之前：</span><span class="sxs-lookup"><span data-stu-id="8fd8c-133">Before using the tools:</span></span>

* <span data-ttu-id="8fd8c-134">瞭解目標和啟始專案之間的差異。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="8fd8c-135">瞭解如何搭配使用工具與 .NET Standard 類別庫。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="8fd8c-136">針對 ASP.NET Core 專案，設定環境。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="8fd8c-137">目標和啟始專案</span><span class="sxs-lookup"><span data-stu-id="8fd8c-137">Target and startup project</span></span>

<span data-ttu-id="8fd8c-138">這些命令會參考*專案*和*啟始專案*。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="8fd8c-139">*專案*也稱為「*目標專案*」，因為它是命令新增或移除檔案的位置。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="8fd8c-140">根據預設，在 [**套件管理員主控台**] 中選取的**預設專案**是目標專案。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="8fd8c-141">您可以使用<nobr>`--project`</nobr>選項，將不同的專案指定為目標專案。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="8fd8c-142">*啟始專案*是工具所建立和執行的專案。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="8fd8c-143">這些工具必須在設計階段執行應用程式程式碼，以取得專案的相關資訊，例如資料庫連接字串和模型的設定。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="8fd8c-144">根據預設，**方案總管**中的**啟始專案**是啟始專案。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="8fd8c-145">您可以使用<nobr>`--startup-project`</nobr>選項，將不同的專案指定為啟始專案。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="8fd8c-146">啟始專案和目標專案通常是相同的專案。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="8fd8c-147">一般情況下，它們是個別的專案，如下所示：</span><span class="sxs-lookup"><span data-stu-id="8fd8c-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="8fd8c-148">EF Core 內容和實體類別位於 .NET Core 類別庫中。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="8fd8c-149">.NET Core 主控台應用程式或 web 應用程式會參考類別庫。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="8fd8c-150">您也可以[將遷移程式碼放在與 EF Core 內容不同的類別庫中](xref:core/managing-schemas/migrations/projects)。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="8fd8c-151">其他目標 framework</span><span class="sxs-lookup"><span data-stu-id="8fd8c-151">Other target frameworks</span></span>

<span data-ttu-id="8fd8c-152">套件管理員主控台工具適用于 .NET Core 或 .NET Framework 專案。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="8fd8c-153">在 .NET Standard 類別庫中具有 EF Core 模型的應用程式，可能沒有 .NET Core 或 .NET Framework 專案。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="8fd8c-154">例如，這適用于 Xamarin 和通用 Windows 平臺應用程式。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="8fd8c-155">在這種情況下，您可以建立 .NET Core 或 .NET Framework 主控台應用程式專案，其目的只是要作為工具的啟始專案。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="8fd8c-156">專案可以是沒有實際程式碼&mdash;的虛擬專案，只需要提供工具的目標。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="8fd8c-157">為什麼需要虛擬專案？</span><span class="sxs-lookup"><span data-stu-id="8fd8c-157">Why is a dummy project required?</span></span> <span data-ttu-id="8fd8c-158">如先前所述，工具必須在設計階段執行應用程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="8fd8c-159">若要這麼做，他們必須使用 .NET Core 或 .NET Framework 執行時間。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="8fd8c-160">當 EF Core 模型位於以 .NET Core 或 .NET Framework 為目標的專案中時，EF Core 工具會從專案借用執行時間。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="8fd8c-161">如果 EF Core 模型位於 .NET Standard 類別庫中，他們就無法這麼做。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="8fd8c-162">.NET Standard 不是實際的 .NET 執行;這是一組 .NET 部署必須支援之 Api 的規格。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="8fd8c-163">因此 .NET Standard 並不足以讓 EF Core 工具執行應用程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="8fd8c-164">您建立用來做為啟始專案的虛設專案，會提供可讓工具載入 .NET Standard 類別庫的具體目標平臺。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="8fd8c-165">ASP.NET Core 環境</span><span class="sxs-lookup"><span data-stu-id="8fd8c-165">ASP.NET Core environment</span></span>

<span data-ttu-id="8fd8c-166">若要指定 ASP.NET Core 專案的環境，請在執行命令之前，先設定**env： ASPNETCORE_ENVIRONMENT** 。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="8fd8c-167">一般參數</span><span class="sxs-lookup"><span data-stu-id="8fd8c-167">Common parameters</span></span>

<span data-ttu-id="8fd8c-168">下表顯示所有 EF Core 命令通用的參數：</span><span class="sxs-lookup"><span data-stu-id="8fd8c-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="8fd8c-169">參數</span><span class="sxs-lookup"><span data-stu-id="8fd8c-169">Parameter</span></span>                 | <span data-ttu-id="8fd8c-170">描述</span><span class="sxs-lookup"><span data-stu-id="8fd8c-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8fd8c-171">-內容\<字串></span><span class="sxs-lookup"><span data-stu-id="8fd8c-171">-Context \<String></span></span>        | <span data-ttu-id="8fd8c-172">要使用的 `DbContext` 類別。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-172">The `DbContext` class to use.</span></span> <span data-ttu-id="8fd8c-173">僅限類別名稱，或使用命名空間完整限定。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="8fd8c-174">如果省略此參數，EF Core 會尋找內容類別。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="8fd8c-175">如果有多個內容類別，則需要此參數。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="8fd8c-176">-專案\<字串></span><span class="sxs-lookup"><span data-stu-id="8fd8c-176">-Project \<String></span></span>        | <span data-ttu-id="8fd8c-177">目標專案。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-177">The target project.</span></span> <span data-ttu-id="8fd8c-178">如果省略此參數，則會使用 [**封裝管理員主控台**] 的**預設專案**做為目標專案。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="8fd8c-179"><nobr>-啟始專案</nobr> \<字串></span><span class="sxs-lookup"><span data-stu-id="8fd8c-179"><nobr>-StartupProject</nobr> \<String></span></span> | <span data-ttu-id="8fd8c-180">啟始專案。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-180">The startup project.</span></span> <span data-ttu-id="8fd8c-181">如果省略此參數，則會使用 [**方案屬性**] 中的**啟始專案**做為目標專案。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="8fd8c-182">-Verbose</span><span class="sxs-lookup"><span data-stu-id="8fd8c-182">-Verbose</span></span>                  | <span data-ttu-id="8fd8c-183">顯示詳細資訊輸出。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="8fd8c-184">若要顯示命令的說明資訊，請使用 PowerShell `Get-Help`的命令。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="8fd8c-185">CoNtext、Project 和啟始專案參數支援 tab 鍵展開。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="8fd8c-186">新增-遷移</span><span class="sxs-lookup"><span data-stu-id="8fd8c-186">Add-Migration</span></span>

<span data-ttu-id="8fd8c-187">加入新的遷移。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-187">Adds a new migration.</span></span>

<span data-ttu-id="8fd8c-188">參數：</span><span class="sxs-lookup"><span data-stu-id="8fd8c-188">Parameters:</span></span>

| <span data-ttu-id="8fd8c-189">參數</span><span class="sxs-lookup"><span data-stu-id="8fd8c-189">Parameter</span></span>                         | <span data-ttu-id="8fd8c-190">描述</span><span class="sxs-lookup"><span data-stu-id="8fd8c-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8fd8c-191"><nobr>-Name \<String><nobr></span><span class="sxs-lookup"><span data-stu-id="8fd8c-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="8fd8c-192">遷移的名稱。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-192">The name of the migration.</span></span> <span data-ttu-id="8fd8c-193">這是位置參數，而且是必要的。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="8fd8c-194"><nobr>-OutputDir \<字串></nobr></span><span class="sxs-lookup"><span data-stu-id="8fd8c-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="8fd8c-195">目錄用來輸出檔案。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-195">The directory use to output the files.</span></span> <span data-ttu-id="8fd8c-196">路徑相對於目標專案目錄。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="8fd8c-197">預設為「遷移」。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-197">Defaults to "Migrations".</span></span> |
| <span data-ttu-id="8fd8c-198"><nobr>-Namespace \<字串></nobr></span><span class="sxs-lookup"><span data-stu-id="8fd8c-198"><nobr>-Namespace \<String></nobr></span></span> | <span data-ttu-id="8fd8c-199">要用於產生之類別的命名空間。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-199">The namespace to use for the generated classes.</span></span> <span data-ttu-id="8fd8c-200">預設為從輸出目錄產生。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-200">Defaults to generated from the output directory.</span></span> |

## <a name="drop-database"></a><span data-ttu-id="8fd8c-201">放置資料庫</span><span class="sxs-lookup"><span data-stu-id="8fd8c-201">Drop-Database</span></span>

<span data-ttu-id="8fd8c-202">卸除資料庫。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-202">Drops the database.</span></span>

<span data-ttu-id="8fd8c-203">參數：</span><span class="sxs-lookup"><span data-stu-id="8fd8c-203">Parameters:</span></span>

| <span data-ttu-id="8fd8c-204">參數</span><span class="sxs-lookup"><span data-stu-id="8fd8c-204">Parameter</span></span> | <span data-ttu-id="8fd8c-205">描述</span><span class="sxs-lookup"><span data-stu-id="8fd8c-205">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="8fd8c-206">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="8fd8c-206">-WhatIf</span></span>   | <span data-ttu-id="8fd8c-207">顯示要捨棄的資料庫，但不要卸載它。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-207">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="8fd8c-208">DbCoNtext</span><span class="sxs-lookup"><span data-stu-id="8fd8c-208">Get-DbContext</span></span>

<span data-ttu-id="8fd8c-209">取得類型的`DbContext`相關資訊。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-209">Gets information about a `DbContext` type.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="8fd8c-210">移除移轉</span><span class="sxs-lookup"><span data-stu-id="8fd8c-210">Remove-Migration</span></span>

<span data-ttu-id="8fd8c-211">移除上一次的遷移作業（復原已針對遷移完成的程式碼變更）。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-211">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="8fd8c-212">參數：</span><span class="sxs-lookup"><span data-stu-id="8fd8c-212">Parameters:</span></span>

| <span data-ttu-id="8fd8c-213">參數</span><span class="sxs-lookup"><span data-stu-id="8fd8c-213">Parameter</span></span> | <span data-ttu-id="8fd8c-214">描述</span><span class="sxs-lookup"><span data-stu-id="8fd8c-214">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="8fd8c-215">-Force</span><span class="sxs-lookup"><span data-stu-id="8fd8c-215">-Force</span></span>    | <span data-ttu-id="8fd8c-216">還原遷移（復原已套用至資料庫的變更）。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-216">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="8fd8c-217">Scaffold-DbCoNtext</span><span class="sxs-lookup"><span data-stu-id="8fd8c-217">Scaffold-DbContext</span></span>

<span data-ttu-id="8fd8c-218">為資料庫的`DbContext`和實體類型產生程式碼。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-218">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="8fd8c-219">為了讓`Scaffold-DbContext`產生實體類型，資料庫資料表必須有主鍵。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-219">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="8fd8c-220">參數：</span><span class="sxs-lookup"><span data-stu-id="8fd8c-220">Parameters:</span></span>

| <span data-ttu-id="8fd8c-221">參數</span><span class="sxs-lookup"><span data-stu-id="8fd8c-221">Parameter</span></span>                          | <span data-ttu-id="8fd8c-222">描述</span><span class="sxs-lookup"><span data-stu-id="8fd8c-222">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8fd8c-223"><nobr>-連接\<字串></nobr></span><span class="sxs-lookup"><span data-stu-id="8fd8c-223"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="8fd8c-224">資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-224">The connection string to the database.</span></span> <span data-ttu-id="8fd8c-225">針對 ASP.NET Core 2.x 專案，此值可以是*name =\<連接字串的名稱>*。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-225">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="8fd8c-226">在此情況下，此名稱來自針對專案所設定的設定來源。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-226">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="8fd8c-227">這是位置參數，而且是必要的。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-227">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="8fd8c-228"><nobr>-提供\<者字串></nobr></span><span class="sxs-lookup"><span data-stu-id="8fd8c-228"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="8fd8c-229">要使用的提供者。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-229">The provider to use.</span></span> <span data-ttu-id="8fd8c-230">這通常是 NuGet 套件的名稱，例如： `Microsoft.EntityFrameworkCore.SqlServer`。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-230">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="8fd8c-231">這是位置參數，而且是必要的。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-231">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="8fd8c-232">-OutputDir \<字串></span><span class="sxs-lookup"><span data-stu-id="8fd8c-232">-OutputDir \<String></span></span>               | <span data-ttu-id="8fd8c-233">要用來放置檔案的目錄。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-233">The directory to put files in.</span></span> <span data-ttu-id="8fd8c-234">路徑相對於專案目錄。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="8fd8c-235">-CoNtextDir \<字串></span><span class="sxs-lookup"><span data-stu-id="8fd8c-235">-ContextDir \<String></span></span>              | <span data-ttu-id="8fd8c-236">要放置`DbContext`檔案的目錄。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-236">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="8fd8c-237">路徑相對於專案目錄。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-237">Paths are relative to the project directory.</span></span>                                                                                                                                                               |
| <span data-ttu-id="8fd8c-238">-Namespace \<字串></span><span class="sxs-lookup"><span data-stu-id="8fd8c-238">-Namespace \<String></span></span>               | <span data-ttu-id="8fd8c-239">要用於所有產生之類別的命名空間。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-239">The namespace to use for all generated classes.</span></span> <span data-ttu-id="8fd8c-240">預設為從根命名空間和輸出目錄產生。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-240">Defaults to generated from the root namespace and the output directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="8fd8c-241">-CoNtextNamespace \<字串></span><span class="sxs-lookup"><span data-stu-id="8fd8c-241">-ContextNamespace \<String></span></span>        | <span data-ttu-id="8fd8c-242">要用於所產生`DbContext`類別的命名空間。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-242">The namespace to use for the generated `DbContext` class.</span></span> <span data-ttu-id="8fd8c-243">注意：覆`-Namespace`寫。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-243">Note: overrides `-Namespace`.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="8fd8c-244">-內容\<字串></span><span class="sxs-lookup"><span data-stu-id="8fd8c-244">-Context \<String></span></span>                 | <span data-ttu-id="8fd8c-245">要產生的`DbContext`類別名稱。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-245">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="8fd8c-246">-架構\<字串 [] ></span><span class="sxs-lookup"><span data-stu-id="8fd8c-246">-Schemas \<String[]></span></span>               | <span data-ttu-id="8fd8c-247">要為其產生實體類型的資料表架構。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-247">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="8fd8c-248">如果省略此參數，則會包含所有架構。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-248">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="8fd8c-249">-Tables \<String [] ></span><span class="sxs-lookup"><span data-stu-id="8fd8c-249">-Tables \<String[]></span></span>                | <span data-ttu-id="8fd8c-250">要為其產生實體類型的資料表。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-250">The tables to generate entity types for.</span></span> <span data-ttu-id="8fd8c-251">如果省略此參數，則會包含所有資料表。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-251">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="8fd8c-252">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="8fd8c-252">-DataAnnotations</span></span>                   | <span data-ttu-id="8fd8c-253">使用屬性來設定模型（可能的話）。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-253">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="8fd8c-254">如果省略此參數，則只會使用 Fluent API。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-254">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="8fd8c-255">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="8fd8c-255">-UseDatabaseNames</span></span>                  | <span data-ttu-id="8fd8c-256">使用資料表和資料行名稱，就像在資料庫中所出現的一樣。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-256">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="8fd8c-257">如果省略這個參數，資料庫名稱就會變更，以更符合 c # 名稱樣式慣例。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-257">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="8fd8c-258">-Force</span><span class="sxs-lookup"><span data-stu-id="8fd8c-258">-Force</span></span>                             | <span data-ttu-id="8fd8c-259">覆寫現有檔案。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-259">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="8fd8c-260">範例：</span><span class="sxs-lookup"><span data-stu-id="8fd8c-260">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="8fd8c-261">僅 scaffold 選取的資料表，並在具有指定之名稱和命名空間的個別資料夾中建立內容的範例：</span><span class="sxs-lookup"><span data-stu-id="8fd8c-261">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name and namespace:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext -ContextNamespace New.Namespace
```

## <a name="script-migration"></a><span data-ttu-id="8fd8c-262">腳本-遷移</span><span class="sxs-lookup"><span data-stu-id="8fd8c-262">Script-Migration</span></span>

<span data-ttu-id="8fd8c-263">產生 SQL 腳本，將所選遷移的所有變更套用到另一個選取的遷移。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-263">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="8fd8c-264">參數：</span><span class="sxs-lookup"><span data-stu-id="8fd8c-264">Parameters:</span></span>

| <span data-ttu-id="8fd8c-265">參數</span><span class="sxs-lookup"><span data-stu-id="8fd8c-265">Parameter</span></span>                | <span data-ttu-id="8fd8c-266">描述</span><span class="sxs-lookup"><span data-stu-id="8fd8c-266">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8fd8c-267">*-從* \<字串></span><span class="sxs-lookup"><span data-stu-id="8fd8c-267">*-From* \<String></span></span>        | <span data-ttu-id="8fd8c-268">開始的遷移。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-268">The starting migration.</span></span> <span data-ttu-id="8fd8c-269">可以依名稱或識別碼來識別遷移。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-269">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="8fd8c-270">數位0是特殊案例，表示*第一次遷移之前*。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-270">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="8fd8c-271">預設為 0。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-271">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="8fd8c-272">*-到* \<字串></span><span class="sxs-lookup"><span data-stu-id="8fd8c-272">*-To* \<String></span></span>          | <span data-ttu-id="8fd8c-273">結束遷移。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-273">The ending migration.</span></span> <span data-ttu-id="8fd8c-274">預設為上次遷移。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-274">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="8fd8c-275"><nobr>-等冪</nobr></span><span class="sxs-lookup"><span data-stu-id="8fd8c-275"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="8fd8c-276">產生可在任何遷移的資料庫上使用的腳本。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-276">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="8fd8c-277">-輸出\<字串></span><span class="sxs-lookup"><span data-stu-id="8fd8c-277">-Output \<String></span></span>        | <span data-ttu-id="8fd8c-278">要寫入結果的檔案。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-278">The file to write the result to.</span></span> <span data-ttu-id="8fd8c-279">如果省略此參數，則會在建立應用程式的執行時間檔案所在的相同資料夾中建立具有所產生名稱的檔案，例如： */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-279">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="8fd8c-280">To、From 和 Output 參數支援 tab 鍵展開。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-280">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="8fd8c-281">下列範例會使用遷移名稱來建立 InitialCreate 遷移的腳本。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-281">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="8fd8c-282">下列範例會使用遷移識別碼，針對 InitialCreate 遷移之後的所有遷移建立腳本。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-282">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="8fd8c-283">更新-資料庫</span><span class="sxs-lookup"><span data-stu-id="8fd8c-283">Update-Database</span></span>

<span data-ttu-id="8fd8c-284">將資料庫更新為上次遷移或指定的遷移。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-284">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="8fd8c-285">參數</span><span class="sxs-lookup"><span data-stu-id="8fd8c-285">Parameter</span></span>                           | <span data-ttu-id="8fd8c-286">描述</span><span class="sxs-lookup"><span data-stu-id="8fd8c-286">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8fd8c-287"><nobr>*-遷移* \<字串></nobr></span><span class="sxs-lookup"><span data-stu-id="8fd8c-287"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="8fd8c-288">目標遷移。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-288">The target migration.</span></span> <span data-ttu-id="8fd8c-289">可以依名稱或識別碼來識別遷移。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-289">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="8fd8c-290">數位0是特殊案例，表示*第一次遷移之前*，並會還原所有遷移。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-290">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="8fd8c-291">如果未指定任何遷移，此命令會預設為上次遷移。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-291">If no migration is specified, the command defaults to the last migration.</span></span> |
| <span data-ttu-id="8fd8c-292"><nobr>-連接\<字串></nobr></span><span class="sxs-lookup"><span data-stu-id="8fd8c-292"><nobr>-Connection \<String></nobr></span></span>  | <span data-ttu-id="8fd8c-293">資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-293">The connection string to the database.</span></span> <span data-ttu-id="8fd8c-294">預設為或`AddDbContext` `OnConfiguring`中指定的值。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-294">Defaults to the one specified in `AddDbContext` or `OnConfiguring`.</span></span> |

> [!TIP]
> <span data-ttu-id="8fd8c-295">[遷移] 參數支援 tab 鍵展開。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-295">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="8fd8c-296">下列範例會還原所有的遷移。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-296">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="8fd8c-297">下列範例會將資料庫更新為指定的遷移。</span><span class="sxs-lookup"><span data-stu-id="8fd8c-297">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="8fd8c-298">第一個使用遷移名稱，第二個使用遷移識別碼和指定的連接：</span><span class="sxs-lookup"><span data-stu-id="8fd8c-298">The first uses the migration name and the second uses the migration ID and a specified connection:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate -Connection your_connection_string
```

## <a name="additional-resources"></a><span data-ttu-id="8fd8c-299">其他資源</span><span class="sxs-lookup"><span data-stu-id="8fd8c-299">Additional resources</span></span>

* [<span data-ttu-id="8fd8c-300">移轉</span><span class="sxs-lookup"><span data-stu-id="8fd8c-300">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="8fd8c-301">反向工程</span><span class="sxs-lookup"><span data-stu-id="8fd8c-301">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
